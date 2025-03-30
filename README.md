<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Message Sender - Управление рассылками</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        .sidebar-item.active {
            @apply bg-gradient-to-r from-blue-100 to-gray-100 font-semibold text-blue-600;
        }
        .message-editor {
            @apply min-h-[200px] border border-gray-300 rounded-lg p-3 focus:outline-none focus:ring-2 focus:ring-blue-500;
        }
        .campaign-status-draft {
            @apply bg-gray-300 text-gray-800 px-2 py-1 rounded-full text-xs;
        }
        .campaign-status-scheduled {
            @apply bg-blue-200 text-blue-800 px-2 py-1 rounded-full text-xs;
        }
        .campaign-status-completed {
            @apply bg-green-200 text-green-800 px-2 py-1 rounded-full text-xs;
        }
        .campaign-status-cancelled {
            @apply bg-red-200 text-red-800 px-2 py-1 rounded-full text-xs;
        }
        .card-hover {
            @apply transition duration-300 hover:shadow-lg hover:-translate-y-1;
        }
        .btn-hover {
            @apply transition duration-200 hover:bg-blue-50 hover:text-blue-700;
        }
        .sidebar {
            transition: transform 0.3s ease-in-out;
        }
        .sidebar-hidden {
            transform: translateX(-100%);
        }
        @media (max-width: 768px) {
            .sidebar {
                @apply fixed inset-y-0 left-0 z-50 w-64;
            }
        }
    </style>
</head>
<body class="bg-gray-100 font-sans antialiased">
    <div class="flex h-screen overflow-hidden">
        <!-- Сайдбар -->
        <div class="fixed inset-y-0 left-0 w-64 bg-white shadow-lg sidebar md:relative md:translate-x-0 sidebar-hidden" id="sidebar">
            <div class="flex flex-col h-full">
                <div class="flex items-center h-16 px-4 border-b bg-gradient-to-r from-blue-500 to-blue-600 text-white">
                    <i class="fas fa-paper-plane mr-2 text-xl"></i>
                    <span class="text-xl font-semibold">Message Sender</span>
                </div>
                <nav class="flex-1 px-2 py-4 space-y-2 overflow-y-auto">
                    <a href="#" class="sidebar-item active flex items-center px-4 py-2 rounded-lg">
                        <i class="fas fa-home mr-3"></i> Главная
                    </a>
                    <a href="#" class="sidebar-item flex items-center px-4 py-2 rounded-lg">
                        <i class="fas fa-users mr-3"></i> Получатели
                    </a>
                    <a href="#" class="sidebar-item flex items-center px-4 py-2 rounded-lg">
                        <i class="fas fa-envelope mr-3"></i> Сообщения
                    </a>
                    <a href="#" class="sidebar-item flex items-center px-4 py-2 rounded-lg">
                        <i class="fas fa-calendar-alt mr-3"></i> Планирование
                    </a>
                    <a href="#" class="sidebar-item flex items-center px-4 py-2 rounded-lg">
                        <i class="fas fa-chart-bar mr-3"></i> Статистика
                    </a>
                </nav>
                <div class="p-4 border-t">
                    <div class="flex items-center">
                        <i class="fas fa-user-circle text-gray-500 text-2xl mr-3"></i>
                        <div>
                            <p class="text-sm font-medium text-gray-800">Иван Петров</p>
                            <p class="text-xs text-gray-500">Администратор</p>
                        </div>
                    </div>
                </div>
            </div>
        </div>

        <!-- Основное содержимое -->
        <div class="flex flex-col flex-1 overflow-hidden">
            <!-- Шапка -->
            <header class="bg-white shadow-md">
                <div class="flex items-center justify-between px-4 py-3">
                    <div class="flex items-center">
                        <button class="md:hidden text-gray-600 focus:outline-none" id="toggleSidebar">
                            <i class="fas fa-bars text-xl"></i>
                        </button>
                        <h1 class="ml-3 text-lg font-semibold text-gray-800">Главная панель</h1>
                    </div>
                    <div class="flex items-center space-x-4">
                        <button class="p-2 text-gray-600 hover:text-blue-600">
                            <i class="fas fa-bell"></i>
                        </button>
                        <button class="p-2 text-gray-600 hover:text-blue-600">
                            <i class="fas fa-cog"></i>
                        </button>
                    </div>
                </div>
            </header>

            <!-- Контент -->
            <main class="flex-1 overflow-y-auto p-6">
                <!-- Статистика -->
                <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-6 mb-8">
                    <div class="bg-white p-6 rounded-xl shadow card-hover">
                        <h3 class="text-sm font-medium text-gray-500">Всего получателей</h3>
                        <p class="mt-2 text-3xl font-bold text-gray-800">1,248</p>
                    </div>
                    <div class="bg-white p-6 rounded-xl shadow card-hover">
                        <h3 class="text-sm font-medium text-gray-500">Всего групп</h3>
                        <p class="mt-2 text-3xl font-bold text-gray-800">24</p>
                    </div>
                    <div class="bg-white p-6 rounded-xl shadow card-hover">
                        <h3 class="text-sm font-medium text-gray-500">Шаблонов сообщений</h3>
                        <p class="mt-2 text-3xl font-bold text-gray-800">15</p>
                    </div>
                    <div class="bg-white p-6 rounded-xl shadow card-hover">
                        <h3 class="text-sm font-medium text-gray-500">Запланировано рассылок</h3>
                        <p class="mt-2 text-3xl font-bold text-gray-800">8</p>
                    </div>
                </div>

                <!-- Быстрые действия -->
                <div class="bg-white p-6 rounded-xl shadow mb-8">
                    <h2 class="text-xl font-semibold text-gray-800 mb-4">Быстрые действия</h2>
                    <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4">
                        <button class="flex flex-col items-center justify-center p-4 border rounded-lg btn-hover">
                            <i class="fas fa-user-plus text-blue-500 text-2xl mb-2"></i>
                            <span class="text-gray-700">Добавить получателя</span>
                        </button>
                        <button class="flex flex-col items-center justify-center p-4 border rounded-lg btn-hover">
                            <i class="fas fa-users text-blue-500 text-2xl mb-2"></i>
                            <span class="text-gray-700">Создать группу</span>
                        </button>
                        <button class="flex flex-col items-center justify-center p-4 border rounded-lg btn-hover">
                            <i class="fas fa-envelope text-blue-500 text-2xl mb-2"></i>
                            <span class="text-gray-700">Новый шаблон</span>
                        </button>
                        <button class="flex flex-col items-center justify-center p-4 border rounded-lg btn-hover">
                            <i class="fas fa-calendar-plus text-blue-500 text-2xl mb-2"></i>
                            <span class="text-gray-700">Запланировать рассылку</span>
                        </button>
                    </div>
                </div>

                <!-- Последние рассылки -->
                <div class="bg-white p-6 rounded-xl shadow">
                    <div class="flex justify-between items-center mb-4">
                        <h2 class="text-xl font-semibold text-gray-800">Последние рассылки</h2>
                        <a href="#" class="text-sm text-blue-500 hover:underline">Показать все</a>
                    </div>
                    <div class="overflow-x-auto">
                        <table class="min-w-full divide-y divide-gray-200">
                            <thead class="bg-gray-50">
                                <tr>
                                    <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Название</th>
Вот улучшенный код в одном файле index.html, готовый для копирования и деплоя через GitHub Pages. Я включил все стили, JavaScript и структуру в единый файл, как вы попросили. Инструкции по запуску через GitHub остаются такими же, просто используйте этот код как единственный файл.<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Message Sender - Управление рассылками</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        .sidebar-item.active {
            @apply bg-gradient-to-r from-blue-100 to-gray-100 font-semibold text-blue-600;
        }
        .message-editor {
            @apply min-h-[200px] border border-gray-300 rounded-lg p-3 focus:outline-none focus:ring-2 focus:ring-blue-500;
        }
        .campaign-status-draft {
            @apply bg-gray-300 text-gray-800 px-2 py-1 rounded-full text-xs;
        }
        .campaign-status-scheduled {
            @apply bg-blue-200 text-blue-800 px-2 py-1 rounded-full text-xs;
        }
        .campaign-status-completed {
            @apply bg-green-200 text-green-800 px-2 py-1 rounded-full text-xs;
        }
        .campaign-status-cancelled {
            @apply bg-red-200 text-red-800 px-2 py-1 rounded-full text-xs;
        }
        .card-hover {
            @apply transition duration-300 hover:shadow-lg hover:-translate-y-1;
        }
        .btn-hover {
            @apply transition duration-200 hover:bg-blue-50 hover:text-blue-700;
        }
        .sidebar {
            transition: transform 0.3s ease-in-out;
        }
        .sidebar-hidden {
            transform: translateX(-100%);
        }
        @media (max-width: 768px) {
            .sidebar {
                @apply fixed inset-y-0 left-0 z-50 w-64;
            }
        }
    </style>
</head>
<body class="bg-gray-100 font-sans antialiased">
    <div class="flex h-screen overflow-hidden">
        <!-- Сайдбар -->
        <div class="fixed inset-y-0 left-0 w-64 bg-white shadow-lg sidebar md:relative md:translate-x-0 sidebar-hidden" id="sidebar">
            <div class="flex flex-col h-full">
                <div class="flex items-center h-16 px-4 border-b bg-gradient-to-r from-blue-500 to-blue-600 text-white">
                    <i class="fas fa-paper-plane mr-2 text-xl"></i>
                    <span class="text-xl font-semibold">Message Sender</span>
                </div>
                <nav class="flex-1 px-2 py-4 space-y-2 overflow-y-auto">
                    <a href="#" class="sidebar-item active flex items-center px-4 py-2 rounded-lg">
                        <i class="fas fa-home mr-3"></i> Главная
                    </a>
                    <a href="#" class="sidebar-item flex items-center px-4 py-2 rounded-lg">
                        <i class="fas fa-users mr-3"></i> Получатели
                    </a>
                    <a href="#" class="sidebar-item flex items-center px-4 py-2 rounded-lg">
                        <i class="fas fa-envelope mr-3"></i> Сообщения
                    </a>
                    <a href="#" class="sidebar-item flex items-center px-4 py-2 rounded-lg">
                        <i class="fas fa-calendar-alt mr-3"></i> Планирование
                    </a>
                    <a href="#" class="sidebar-item flex items-center px-4 py-2 rounded-lg">
                        <i class="fas fa-chart-bar mr-3"></i> Статистика
                    </a>
                </nav>
                <div class="p-4 border-t">
                    <div class="flex items-center">
                        <i class="fas fa-user-circle text-gray-500 text-2xl mr-3"></i>
                        <div>
                            <p class="text-sm font-medium text-gray-800">Иван Петров</p>
                            <p class="text-xs text-gray-500">Администратор</p>
                        </div>
                    </div>
                </div>
            </div>
        </div>

        <!-- Основное содержимое -->
        <div class="flex flex-col flex-1 overflow-hidden">
            <!-- Шапка -->
            <header class="bg-white shadow-md">
                <div class="flex items-center justify-between px-4 py-3">
                    <div class="flex items-center">
                        <button class="md:hidden text-gray-600 focus:outline-none" id="toggleSidebar">
                            <i class="fas fa-bars text-xl"></i>
                        </button>
                        <h1 class="ml-3 text-lg font-semibold text-gray-800">Главная панель</h1>
                    </div>
                    <div class="flex items-center space-x-4">
                        <button class="p-2 text-gray-600 hover:text-blue-600">
                            <i class="fas fa-bell"></i>
                        </button>
                        <button class="p-2 text-gray-600 hover:text-blue-600">
                            <i class="fas fa-cog"></i>
                        </button>
                    </div>
                </div>
            </header>

            <!-- Контент -->
            <main class="flex-1 overflow-y-auto p-6">
                <!-- Статистика -->
                <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-6 mb-8">
                    <div class="bg-white p-6 rounded-xl shadow card-hover">
                        <h3 class="text-sm font-medium text-gray-500">Всего получателей</h3>
                        <p class="mt-2 text-3xl font-bold text-gray-800">1,248</p>
                    </div>
                    <div class="bg-white p-6 rounded-xl shadow card-hover">
                        <h3 class="text-sm font-medium text-gray-500">Всего групп</h3>
                        <p class="mt-2 text-3xl font-bold text-gray-800">24</p>
                    </div>
                    <div class="bg-white p-6 rounded-xl shadow card-hover">
                        <h3 class="text-sm font-medium text-gray-500">Шаблонов сообщений</h3>
                        <p class="mt-2 text-3xl font-bold text-gray-800">15</p>
                    </div>
                    <div class="bg-white p-6 rounded-xl shadow card-hover">
                        <h3 class="text-sm font-medium text-gray-500">Запланировано рассылок</h3>
                        <p class="mt-2 text-3xl font-bold text-gray-800">8</p>
                    </div>
                </div>

                <!-- Быстрые действия -->
                <div class="bg-white p-6 rounded-xl shadow mb-8">
                    <h2 class="text-xl font-semibold text-gray-800 mb-4">Быстрые действия</h2>
                    <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4">
                        <button class="flex flex-col items-center justify-center p-4 border rounded-lg btn-hover">
                            <i class="fas fa-user-plus text-blue-500 text-2xl mb-2"></i>
                            <span class="text-gray-700">Добавить получателя</span>
                        </button>
                        <button class="flex flex-col items-center justify-center p-4 border rounded-lg btn-hover">
                            <i class="fas fa-users text-blue-500 text-2xl mb-2"></i>
                            <span class="text-gray-700">Создать группу</span>
                        </button>
                        <button class="flex flex-col items-center justify-center p-4 border rounded-lg btn-hover">
                            <i class="fas fa-envelope text-blue-500 text-2xl mb-2"></i>
                            <span class="text-gray-700">Новый шаблон</span>
                        </button>
                        <button class="flex flex-col items-center justify-center p-4 border rounded-lg btn-hover">
                            <i class="fas fa-calendar-plus text-blue-500 text-2xl mb-2"></i>
                            <span class="text-gray-700">Запланировать рассылку</span>
                        </button>
                    </div>
                </div>

                <!-- Последние рассылки -->
                <div class="bg-white p-6 rounded-xl shadow">
                    <div class="flex justify-between items-center mb-4">
                        <h2 class="text-xl font-semibold text-gray-800">Последние рассылки</h2>
                        <a href="#" class="text-sm text-blue-500 hover:underline">Показать все</a>
                    </div>
                    <div class="overflow-x-auto">
                        <table class="min-w-full divide-y divide-gray-200">
                            <thead class="bg-gray-50">
                                <tr>
                                    <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Название</th>
                                    <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Группа</th>
                                    <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Дата</th>
                                    <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Статус</th>
                                    <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Действия</th>
                                </tr>
                            </thead>
                            <tbody class="divide-y divide-gray-200">
                                <tr class="hover:bg-gray-50">
                                    <td class="px-6 py-4 text-sm font-medium text-gray-900">Акция Весна 2025</td>
                                    <td class="px-6 py-4 text-sm text-gray-500">Клиенты VIP</td>
                                    <td class="px-6 py-4 text-sm text-gray-500">28.03.2025</td>
                                    <td class="px-6 py-4"><span class="campaign-status-completed">Завершено</span></td>
                                    <td class="px-6 py-4 text-sm"><a href="#" class="text-blue-500 hover:underline">Просмотр</a></td>
                                </tr>
                                <tr class="hover:bg-gray-50">
                                    <td class="px-6 py-4 text-sm font-medium text-gray-900">Напоминание о платеже</td>
                                    <td class="px-6 py-4 text-sm text-gray-500">Все пользователи</td>
                                    <td class="px-6 py-4 text-sm text-gray-500">31.03.2025</td>
                                    <td class="px-6 py-4"><span class="campaign-status-scheduled">Запланировано</span></td>
                                    <td class="px-6 py-4 text-sm"><a href="#" class="text-blue-500 hover:underline">Редактировать</a></td>
                                </tr>
                                <tr class="hover:bg-gray-50">
                                    <td class="px-6 py-4 text-sm font-medium text-gray-900">Тестовая рассылка</td>
                                    <td class="px-6 py-4 text-sm text-gray-500">Тестовая группа</td>
                                    <td class="px-6 py-4 text-sm text-gray-500">25.03.2025</td>
                                    <td class="px-6 py-4"><span class="campaign-status-cancelled">Отменено</span></td>
                                    <td class="px-6 py-4 text-sm"><a href="#" class="text-blue-500 hover:underline">Удалить</a></td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                </div>
            </main>
        </div>
    </div>

    <!-- JavaScript для интерактивности -->
    <script>
        // Переключение сайдбара на мобильных устройствах
        const toggleSidebar = document.getElementById('toggleSidebar');
        const sidebar = document.getElementById('sidebar');
        toggleSidebar.addEventListener('click', () => {
            sidebar.classList.toggle('sidebar-hidden');
        });

        // Активный пункт меню
        const sidebarItems = document.querySelectorAll('.sidebar-item');
        sidebarItems.forEach(item => {
            item.addEventListener('click', () => {
                sidebarItems.forEach(i => i.classList.remove('active'));
                item.classList.add('active');
            });
        });
    </script>
</body>
Вот улучшенный код в одном файле index.html, готовый для копирования и деплоя через GitHub Pages. Я включил все стили, JavaScript и структуру в единый файл, как вы попросили. Инструкции по запуску через GitHub остаются такими же, просто используйте этот код как единственный файл.<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Message Sender - Управление рассылками</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        .sidebar-item.active {
            @apply bg-gradient-to-r from-blue-100 to-gray-100 font-semibold text-blue-600;
        }
        .message-editor {
            @apply min-h-[200px] border border-gray-300 rounded-lg p-3 focus:outline-none focus:ring-2 focus:ring-blue-500;
        }
        .campaign-status-draft {
            @apply bg-gray-300 text-gray-800 px-2 py-1 rounded-full text-xs;
        }
        .campaign-status-scheduled {
            @apply bg-blue-200 text-blue-800 px-2 py-1 rounded-full text-xs;
        }
        .campaign-status-completed {
            @apply bg-green-200 text-green-800 px-2 py-1 rounded-full text-xs;
        }
        .campaign-status-cancelled {
            @apply bg-red-200 text-red-800 px-2 py-1 rounded-full text-xs;
        }
        .card-hover {
            @apply transition duration-300 hover:shadow-lg hover:-translate-y-1;
        }
        .btn-hover {
            @apply transition duration-200 hover:bg-blue-50 hover:text-blue-700;
        }
        .sidebar {
            transition: transform 0.3s ease-in-out;
        }
        .sidebar-hidden {
            transform: translateX(-100%);
        }
        @media (max-width: 768px) {
            .sidebar {
                @apply fixed inset-y-0 left-0 z-50 w-64;
            }
        }
    </style>
</head>
<body class="bg-gray-100 font-sans antialiased">
    <div class="flex h-screen overflow-hidden">
        <!-- Сайдбар -->
        <div class="fixed inset-y-0 left-0 w-64 bg-white shadow-lg sidebar md:relative md:translate-x-0 sidebar-hidden" id="sidebar">
            <div class="flex flex-col h-full">
                <div class="flex items-center h-16 px-4 border-b bg-gradient-to-r from-blue-500 to-blue-600 text-white">
                    <i class="fas fa-paper-plane mr-2 text-xl"></i>
                    <span class="text-xl font-semibold">Message Sender</span>
                </div>
                <nav class="flex-1 px-2 py-4 space-y-2 overflow-y-auto">
                    <a href="#" class="sidebar-item active flex items-center px-4 py-2 rounded-lg">
                        <i class="fas fa-home mr-3"></i> Главная
                    </a>
                    <a href="#" class="sidebar-item flex items-center px-4 py-2 rounded-lg">
                        <i class="fas fa-users mr-3"></i> Получатели
                    </a>
                    <a href="#" class="sidebar-item flex items-center px-4 py-2 rounded-lg">
                        <i class="fas fa-envelope mr-3"></i> Сообщения
                    </a>
                    <a href="#" class="sidebar-item flex items-center px-4 py-2 rounded-lg">
                        <i class="fas fa-calendar-alt mr-3"></i> Планирование
                    </a>
                    <a href="#" class="sidebar-item flex items-center px-4 py-2 rounded-lg">
                        <i class="fas fa-chart-bar mr-3"></i> Статистика
                    </a>
                </nav>
                <div class="p-4 border-t">
                    <div class="flex items-center">
                        <i class="fas fa-user-circle text-gray-500 text-2xl mr-3"></i>
                        <div>
                            <p class="text-sm font-medium text-gray-800">Иван Петров</p>
                            <p class="text-xs text-gray-500">Администратор</p>
                        </div>
                    </div>
                </div>
            </div>
        </div>

        <!-- Основное содержимое -->
        <div class="flex flex-col flex-1 overflow-hidden">
            <!-- Шапка -->
            <header class="bg-white shadow-md">
                <div class="flex items-center justify-between px-4 py-3">
                    <div class="flex items-center">
                        <button class="md:hidden text-gray-600 focus:outline-none" id="toggleSidebar">
                            <i class="fas fa-bars text-xl"></i>
                        </button>
                        <h1 class="ml-3 text-lg font-semibold text-gray-800">Главная панель</h1>
                    </div>
                    <div class="flex items-center space-x-4">
                        <button class="p-2 text-gray-600 hover:text-blue-600">
                            <i class="fas fa-bell"></i>
                        </button>
                        <button class="p-2 text-gray-600 hover:text-blue-600">
                            <i class="fas fa-cog"></i>
                        </button>
                    </div>
                </div>
            </header>

            <!-- Контент -->
            <main class="flex-1 overflow-y-auto p-6">
                <!-- Статистика -->
                <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-6 mb-8">
                    <div class="bg-white p-6 rounded-xl shadow card-hover">
                        <h3 class="text-sm font-medium text-gray-500">Всего получателей</h3>
                        <p class="mt-2 text-3xl font-bold text-gray-800">1,248</p>
                    </div>
                    <div class="bg-white p-6 rounded-xl shadow card-hover">
                        <h3 class="text-sm font-medium text-gray-500">Всего групп</h3>
                        <p class="mt-2 text-3xl font-bold text-gray-800">24</p>
                    </div>
                    <div class="bg-white p-6 rounded-xl shadow card-hover">
                        <h3 class="text-sm font-medium text-gray-500">Шаблонов сообщений</h3>
                        <p class="mt-2 text-3xl font-bold text-gray-800">15</p>
                    </div>
                    <div class="bg-white p-6 rounded-xl shadow card-hover">
                        <h3 class="text-sm font-medium text-gray-500">Запланировано рассылок</h3>
                        <p class="mt-2 text-3xl font-bold text-gray-800">8</p>
                    </div>
                </div>

                <!-- Быстрые действия -->
                <div class="bg-white p-6 rounded-xl shadow mb-8">
                    <h2 class="text-xl font-semibold text-gray-800 mb-4">Быстрые действия</h2>
                    <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4">
                        <button class="flex flex-col items-center justify-center p-4 border rounded-lg btn-hover">
                            <i class="fas fa-user-plus text-blue-500 text-2xl mb-2"></i>
                            <span class="text-gray-700">Добавить получателя</span>
                        </button>
                        <button class="flex flex-col items-center justify-center p-4 border rounded-lg btn-hover">
                            <i class="fas fa-users text-blue-500 text-2xl mb-2"></i>
                            <span class="text-gray-700">Создать группу</span>
                        </button>
                        <button class="flex flex-col items-center justify-center p-4 border rounded-lg btn-hover">
                            <i class="fas fa-envelope text-blue-500 text-2xl mb-2"></i>
                            <span class="text-gray-700">Новый шаблон</span>
                        </button>
                        <button class="flex flex-col items-center justify-center p-4 border rounded-lg btn-hover">
                            <i class="fas fa-calendar-plus text-blue-500 text-2xl mb-2"></i>
                            <span class="text-gray-700">Запланировать рассылку</span>
                        </button>
                    </div>
                </div>

                <!-- Последние рассылки -->
                <div class="bg-white p-6 rounded-xl shadow">
                    <div class="flex justify-between items-center mb-4">
                        <h2 class="text-xl font-semibold text-gray-800">Последние рассылки</h2>
                        <a href="#" class="text-sm text-blue-500 hover:underline">Показать все</a>
                    </div>
                    <div class="overflow-x-auto">
                        <table class="min-w-full divide-y divide-gray-200">
                            <thead class="bg-gray-50">
                                <tr>
                                    <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Название</th>
                                    <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Группа</th>
                                    <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Дата</th>
                                    <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Статус</th>
                                    <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Действия</th>
                                </tr>
                            </thead>
                            <tbody class="divide-y divide-gray-200">
                                <tr class="hover:bg-gray-50">
                                    <td class="px-6 py-4 text-sm font-medium text-gray-900">Акция Весна 2025</td>
                                    <td class="px-6 py-4 text-sm text-gray-500">Клиенты VIP</td>
                                    <td class="px-6 py-4 text-sm text-gray-500">28.03.2025</td>
                                    <td class="px-6 py-4"><span class="campaign-status-completed">Завершено</span></td>
                                    <td class="px-6 py-4 text-sm"><a href="#" class="text-blue-500 hover:underline">Просмотр</a></td>
                                </tr>
                                <tr class="hover:bg-gray-50">
                                    <td class="px-6 py-4 text-sm font-medium text-gray-900">Напоминание о платеже</td>
                                    <td class="px-6 py-4 text-sm text-gray-500">Все пользователи</td>
                                    <td class="px-6 py-4 text-sm text-gray-500">31.03.2025</td>
                                    <td class="px-6 py-4"><span class="campaign-status-scheduled">Запланировано</span></td>
                                    <td class="px-6 py-4 text-sm"><a href="#" class="text-blue-500 hover:underline">Редактировать</a></td>
                                </tr>
                                <tr class="hover:bg-gray-50">
                                    <td class="px-6 py-4 text-sm font-medium text-gray-900">Тестовая рассылка</td>
                                    <td class="px-6 py-4 text-sm text-gray-500">Тестовая группа</td>
                                    <td class="px-6 py-4 text-sm text-gray-500">25.03.2025</td>
                                    <td class="px-6 py-4"><span class="campaign-status-cancelled">Отменено</span></td>
                                    <td class="px-6 py-4 text-sm"><a href="#" class="text-blue-500 hover:underline">Удалить</a></td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                </div>
            </main>
        </div>
    </div>

    <!-- JavaScript для интерактивности -->
    <script>
        // Переключение сайдбара на мобильных устройствах
        const toggleSidebar = document.getElementById('toggleSidebar');
        const sidebar = document.getElementById('sidebar');
        toggleSidebar.addEventListener('click', () => {
            sidebar.classList.toggle('sidebar-hidden');
        });

        // Активный пункт меню
        const sidebarItems = document.querySelectorAll('.sidebar-item');
        sidebarItems.forEach(item => {
            item.addEventListener('click', () => {
                sidebarItems.forEach(i => i.classList.remove('active'));
                item.classList.add('active');
            });
        });
    </script>
</body>
</html>Как развернуть на GitHubСоздайте репозиторий:На GitHub нажмите "New repository".Назови его, например, message-sender-dashboard.Создайте публичный репозиторий.Добавьте файл:Скопируйте весь код выше в файл index.html.Загрузите его через интерфейс GitHub:Нажмите "Add file" → "Upload files".Перетащите index.html или выберите его.Нажмите "Commit changes".Или используйте Git:git init
git add index.html
git commit -m "Initial commit"
git remote add origin https://github.com/ВАШ_ЛОГИН/message-sender-dashboard.git
git push -u origin mainНастройте GitHub Pages:Перейдите в настройки репозитория → "Pages".Выберите ветку main и корневую папку (/root).Сохраните. Через пару минут сайт будет доступен по адресу: https://ВАШ_ЛОГИН.github.io/message-sender-dashboard/.Что включеноАдаптивный дизайн с мобильным сайдбаром (скрыт по умолчанию, открывается по кнопке).Интерактивное меню с подсветкой активного пункта.Современные стили с анимациями (hover-эффекты, градиенты).Всё в одном файле для простоты деплоя.Если нужно что-то ещё улучшить или добавить (например, динамические данные), дайте знать!￼Enter                                    <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Дата</th>
                                    <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Статус</th>
                                    <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Действия</th>
                                </tr>
                            </thead>
                            <tbody class="divide-y divide-gray-200">
                                <tr class="hover:bg-gray-50">
                                    <td class="px-6 py-4 text-sm font-medium text-gray-900">Акция Весна 2025</td>
                                    <td class="px-6 py-4 text-sm text-gray-500">Клиенты VIP</td>
                                    <td class="px-6 py-4 text-sm text-gray-500">28.03.2025</td>
                                    <td class="px-6 py-4"><span class="campaign-status-completed">Завершено</span></td>
                                    <td class="px-6 py-4 text-sm"><a href="#" class="text-blue-500 hover:underline">Просмотр</a></td>
                                </tr>
                                <tr class="hover:bg-gray-50">
                              <td class="px-6 py-4 text-sm font-medium text-gray-900">Напоминание о платеже</td>
                                    <td class="px-6 py-4 text-sm text-gray-500">Все пользователи</td>
                                    <td class="px-6 py-4 text-sm text-gray-500">31.03.2025</td>
                                    <td class="px-6 py-4"><span class="campaign-status-scheduled">Запланировано</span></td>
                                    <td class="px-6 py-4 text-sm"><a href="#" class="text-blue-500 hover:underline">Редактировать</a></td>
                                </tr>
                                <tr class="hover:bg-gray-50">
                                    <td class="px-6 py-4 text-sm font-medium text-gray-900">Тестовая рассылка</td>
                                    <td class="px-6 py-4 text-sm text-gray-500">Тестовая группа</td>
                                    <td class="px-6 py-4 text-sm text-gray-500">25.03.2025</td>
                                    <td class="px-6 py-4"><span class="campaign-status-cancelled">Отменено</span></td>
                                    <td class="px-6 py-4 text-sm"><a href="#" class="text-blue-500 hover:underline">Удалить</a></td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                </div>
            </main>
        </div>
    </div>

    <!-- JavaScript для интерактивности -->
    <script>
        // Переключение сайдбара на мобильных устройствах
        const toggleSidebar = document.getElementById('toggleSidebar');
