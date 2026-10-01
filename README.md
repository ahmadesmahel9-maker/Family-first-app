# Family-first-app
Home service
<!DOCTYPE html>
<html lang="ckb" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Family First - سیستەمی کۆنتڕۆڵی سەردانیکەران</title>
    <!-- Tailwind CSS بۆ دیزاینی خێرا و ڕێکپۆش -->
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Vazirmatn:wght@300;400;700&display=swap" rel="stylesheet">
    <style>
        body { font-family: 'Vazirmatn', sans-serif; }
    </style>
</head>
<body class="bg-slate-50 text-slate-800 min-h-screen">

    <!-- Header / Nav -->
    <header class="bg-indigo-900 text-white shadow-lg">
        <div class="max-w-7xl mx-auto px-4 py-4 flex justify-between items-center">
            <div class="flex items-center space-x-3 space-x-reverse">
                <div class="w-10 h-10 bg-indigo-500 rounded-full flex items-center justify-center font-bold text-xl">F</div>
                <div>
                    <h1 class="font-bold text-lg leading-none">Family First Care</h1>
                    <span class="text-xs text-indigo-200">سیستەمی کۆنتڕۆڵی سەردانیکەران</span>
                </div>
            </div>
            <button onclick="toggleModal()" class="bg-emerald-500 hover:bg-emerald-600 text-white px-4 py-2 rounded-lg font-bold text-sm transition">
                + تۆمارکردنی سەردانی نوێ
            </button>
        </div>
    </header>

    <!-- Main Content -->
    <main class="max-w-7xl mx-auto px-4 py-8">
        
        <!-- Cards Stats / ئامارەکان -->
        <div class="grid grid-cols-1 md:grid-cols-3 gap-6 mb-8">
            <div class="bg-white p-6 rounded-xl border border-slate-200 shadow-sm flex justify-between items-center">
                <div>
                    <p class="text-xs text-slate-500 mb-1">کۆی سەردانەکان</p>
                    <h3 id="total-visits" class="text-3xl font-bold text-slate-800">0</h3>
                </div>
                <div class="p-3 bg-indigo-50 text-indigo-600 rounded-lg">📋</div>
            </div>
            <div class="bg-white p-6 rounded-xl border border-slate-200 shadow-sm flex justify-between items-center">
                <div>
                    <p class="text-xs text-slate-500 mb-1">سەردانەکانی ئەمڕۆ</p>
                    <h3 id="today-visits" class="text-3xl font-bold text-emerald-600">0</h3>
                </div>
                <div class="p-3 bg-emerald-50 text-emerald-600 rounded-lg">📅</div>
            </div>
            <div class="bg-white p-6 rounded-xl border border-slate-200 shadow-sm flex justify-between items-center">
                <div>
                    <p class="text-xs text-slate-500 mb-1">کۆی گشتی داھات (IQD)</p>
                    <h3 id="total-revenue" class="text-3xl font-bold text-indigo-900">0</h3>
                </div>
                <div class="p-3 bg-amber-50 text-amber-600 rounded-lg">💰</div>
            </div>
        </div>

        <!-- Filter & Search / گەڕان -->
        <div class="bg-white p-4 rounded-xl border border-slate-200 shadow-sm mb-6 flex flex-col md:flex-row gap-4 justify-between">
            <input type="text" id="search-input" onkeyup="renderTable()" placeholder="گەڕان بەپێی ناوی نەخۆش یان ژمارە مۆبایل..." class="w-full md:w-1/2 p-2.5 border rounded-lg text-sm focus:outline-none focus:border-indigo-500">
            <select id="filter-service" onchange="renderTable()" class="p-2.5 border rounded-lg text-sm bg-white focus:outline-none focus:border-indigo-500">
                <option value="all">هەموو خزمەتگوزارییەکان</option>
                <option value="Laser">لایزر (Laser)</option>
                <option value="Hijama">کەڵەشاخ (حجامة)</option>
                <option value="Facial">فەیشاڵ / کاربۆن</option>
                <option value="Nursing">پەرستاری عمومی</option>
            </select>
        </div>

        <!-- Table / خشتەی سەردانیکەران -->
        <div class="bg-white rounded-xl border border-slate-200 shadow-sm overflow-hidden">
            <div class="overflow-x-auto">
                <table class="w-full text-right text-sm">
                    <thead class="bg-slate-100 border-b text-slate-600">
                        <tr>
                            <th class="p-4">ناوی نەخۆش</th>
                            <th class="p-4">ژمارە مۆبایل</th>
                            <th class="p-4">گەڕەک / شوێن</th>
                            <th class="p-4">خزمەتگوزاری</th>
                            <th class="p-4">کاتی سەردان</th>
                            <th class="p-4">بڕی پارە (IQD)</th>
                            <th class="p-4">کردارەکان</th>
                        </tr>
                    </thead>
                    <tbody id="visitors-table-body" class="divide-y divide-slate-100">
                        <!-- زانیارییەکان لێرە دروست دەبن -->
                    </tbody>
                </table>
            </div>
        </div>
    </main>

    <!-- Modal Form / فۆڕمی تۆمارکردن -->
    <div id="modal" class="fixed inset-0 bg-slate-900/50 backdrop-blur-sm hidden flex items-center justify-center p-4">
        <div class="bg-white w-full max-w-lg rounded-xl shadow-2xl p-6">
            <div class="flex justify-between items-center mb-4 border-b pb-2">
                <h3 class="font-bold text-lg text-slate-800">تۆمارکردنی سەردانیکی نوێ</h3>
                <button onclick="toggleModal()" class="text-slate-400 hover:text-slate-600 font-bold">✕</button>
            </div>
            <form id="visit-form" onsubmit="saveVisit(event)" class="space-y-4">
                <div>
                    <label class="block text-xs font-bold text-slate-600 mb-1">ناوی تەواوی نەخۆش</label>
                    <input type="text" id="patient-name" required class="w-full p-2.5 border rounded-lg text-sm">
                </div>
                <div class="grid grid-cols-2 gap-4">
                    <div>
                        <label class="block text-xs font-bold text-slate-600 mb-1">ژمارەی مۆبایل</label>
                        <input type="tel" id="patient-phone" required class="w-full p-2.5 border rounded-lg text-sm">
                    </div>
                    <div>
                        <label class="block text-xs font-bold text-slate-600 mb-1">گەڕەک / ناونیشان</label>
                        <input type="text" id="patient-address" required class="w-full p-2.5 border rounded-lg text-sm" placeholder="نموونە: قەیوان سیتی">
                    </div>
                </div>
                <div class="grid grid-cols-2 gap-4">
                    <div>
                        <label class="block text-xs font-bold text-slate-600 mb-1">خزمەتگوزاری</label>
                        <select id="service-type" class="w-full p-2.5 border rounded-lg text-sm">
                            <option value="Laser">لایزر (Laser)</option>
                            <option value="Hijama">کەڵەشاخ (حجامة)</option>
                            <option value="Facial">فەیشاڵ / کاربۆن</option>
                            <option value="Nursing">پەرستاری عمومی</option>
                        </select>
                    </div>
                    <div>
                        <label class="block text-xs font-bold text-slate-600 mb-1">بڕی پارە (IQD)</label>
                        <input type="number" id="visit-price" required class="w-full p-2.5 border rounded-lg text-sm" placeholder="30000">
                    </div>
                </div>
                <div>
                    <label class="block text-xs font-bold text-slate-600 mb-1">ڕێکەوت و کاتی سەردان</label>
                    <input type="datetime-local" id="visit-date" required class="w-full p-2.5 border rounded-lg text-sm">
                </div>
                <div class="flex justify-end space-x-2 space-x-reverse pt-4">
                    <button type="button" onclick="toggleModal()" class="px-4 py-2 bg-slate-200 text-slate-700 rounded-lg text-sm font-bold">پەشیمانبوونەوە</button>
                    <button type="submit" class="px-4 py-2 bg-indigo-900 text-white rounded-lg text-sm font-bold">تۆمارکردن</button>
                </div>
            </form>
        </div>
    </div>

    <!-- JavaScript logic -->
    <script>
        let visits = JSON.parse(localStorage.getItem('ff_visits')) || [];

        function toggleModal() {
            document.getElementById('modal').classList.toggle('hidden');
        }

        function saveVisit(e) {
            e.preventDefault();
            const newVisit = {
                id: Date.now(),
                name: document.getElementById('patient-name').value,
                phone: document.getElementById('patient-phone').value,
                address: document.getElementById('patient-address').value,
                service: document.getElementById('service-type').value,
                price: parseFloat(document.getElementById('visit-price').value),
                date: document.getElementById('visit-date').value
            };

            visits.push(newVisit);
            localStorage.setItem('ff_visits', JSON.stringify(visits));
            document.getElementById('visit-form').reset();
            toggleModal();
            renderTable();
        }

        function deleteVisit(id) {
            if(confirm('ئایا دڵنیایت لە سڕینەوەی ئەم سەردانە؟')) {
                visits = visits.filter(v => v.id !== id);
                localStorage.setItem('ff_visits', JSON.stringify(visits));
                renderTable();
            }
        }

        function renderTable() {
            const tableBody = document.getElementById('visitors-table-body');
            const search = document.getElementById('search-input').value.toLowerCase();
            const filterService = document.getElementById('filter-service').value;

            tableBody.innerHTML = '';
            let totalRev = 0;
            let todayCount = 0;
            const todayStr = new Date().toISOString().split('T')[0];

            const filteredVisits = visits.filter(v => {
                const matchesSearch = v.name.toLowerCase().includes(search) || v.phone.includes(search);
                const matchesService = filterService === 'all' || v.service === filterService;
                return matchesSearch && matchesService;
            });

            filteredVisits.forEach(v => {
                totalRev += v.price;
                if(v.date.startsWith(todayStr)) todayCount++;

                tableBody.innerHTML += `
                    <tr class="hover:bg-slate-50 transition">
                        <td class="p-4 font-bold text-slate-800">${v.name}</td>
                        <td class="p-4 text-slate-600" dir="ltr">${v.phone}</td>
                        <td class="p-4 text-slate-600">${v.address}</td>
                        <td class="p-4"><span class="px-2.5 py-1 rounded-full text-xs font-bold ${getServiceBadge(v.service)}">${v.service}</span></td>
                        <td class="p-4 text-slate-600" dir="ltr">${v.date.replace('T', ' ')}</td>
                        <td class="p-4 font-bold text-indigo-900">${v.price.toLocaleString()} IQD</td>
                        <td class="p-4">
                            <button onclick="deleteVisit(${v.id})" class="text-rose-500 hover:text-rose-700 font-bold text-xs">سڕینەوە</button>
                        </td>
                    </tr>
                `;
            });

            document.getElementById('total-visits').innerText = visits.length;
            document.getElementById('today-visits').innerText = todayCount;
            document.getElementById('total-revenue').innerText = totalRev.toLocaleString();
        }

        function getServiceBadge(service) {
            switch(service) {
                case 'Laser': return 'bg-purple-100 text-purple-700';
                case 'Hijama': return 'bg-rose-100 text-rose-700';
                case 'Facial': return 'bg-amber-100 text-amber-700';
                default: return 'bg-blue-100 text-blue-700';
            }
        }

        // بارکردنی سەرەتایی
        renderTable();
    </script>
</body>
</html>
