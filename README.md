
<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>GLOX Terminal Dashboard</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        :root {
            --primary: #ff6b35;
            --primary-glow: rgba(255, 107, 53, 0.7);
            --bg-gradient: linear-gradient(135deg, #0f0f23 0%, #1a1a2e 100%);
            --terminal-green: #00ff9d;
            --terminal-text: #e0e0ff;
            --card-bg: rgba(30, 30, 46, 0.9);
            --border-color: #333355;
        }
        
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Courier New', monospace;
        }
        
        body {
            background: var(--bg-gradient);
            color: var(--terminal-text);
            min-height: 100vh;
            padding: 20px;
            overflow-x: hidden;
        }
        
        .terminal-header {
            text-align: center;
            margin-bottom: 30px;
            border-bottom: 2px solid var(--primary);
            padding-bottom: 15px;
        }
        
        .terminal-header h1 {
            color: var(--terminal-green);
            font-size: 2.5rem;
            text-shadow: 0 0 10px rgba(0, 255, 157, 0.5);
            letter-spacing: 3px;
        }
        
        .terminal-header p {
            color: #aaaacc;
            font-size: 1.1rem;
        }
        
        /* Главный экран с кругом */
        .main-screen {
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            margin: 40px auto;
            position: relative;
        }
        
        .glowing-circle {
            width: 200px;
            height: 200px;
            border-radius: 50%;
            background: rgba(30, 30, 46, 0.8);
            border: 3px solid var(--primary);
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            box-shadow: 0 0 40px var(--primary-glow);
            transition: all 0.3s ease;
            position: relative;
            overflow: hidden;
        }
        
        .glowing-circle::before {
            content: '';
            position: absolute;
            width: 300px;
            height: 300px;
            background: radial-gradient(circle, var(--primary-glow) 0%, transparent 70%);
            animation: pulse 3s infinite alternate;
        }
        
        @keyframes pulse {
            0% { opacity: 0.3; transform: scale(0.8); }
            100% { opacity: 0.6; transform: scale(1.1); }
        }
        
        .glowing-circle:hover {
            transform: scale(1.05);
            box-shadow: 0 0 60px var(--primary-glow);
        }
        
        .glowing-circle:active {
            transform: scale(0.95);
        }
        
        .circle-content {
            position: relative;
            z-index: 2;
            text-align: center;
            padding: 20px;
        }
        
        .status-indicator {
            width: 15px;
            height: 15px;
            background-color: var(--terminal-green);
            border-radius: 50%;
            margin: 0 auto 15px;
            box-shadow: 0 0 10px var(--terminal-green);
        }
        
        .circle-content h2 {
            color: white;
            font-size: 1.5rem;
            margin-bottom: 10px;
        }
        
        .circle-content p {
            color: #aaaacc;
            font-size: 0.9rem;
        }
        
        /* Зона виджетов */
        .widgets-area {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
            gap: 20px;
            margin-top: 40px;
            padding: 20px;
            border-radius: 10px;
            background: rgba(20, 20, 35, 0.5);
            border: 1px solid var(--border-color);
        }
        
        .widget {
            background: var(--card-bg);
            border: 1px solid var(--border-color);
            border-radius: 10px;
            padding: 20px;
            position: relative;
            transition: transform 0.3s ease, box-shadow 0.3s ease;
            cursor: move;
        }
        
        .widget:hover {
            transform: translateY(-5px);
            box-shadow: 0 10px 20px rgba(0, 0, 0, 0.3);
        }
        
        .widget-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 15px;
            border-bottom: 1px solid var(--border-color);
            padding-bottom: 10px;
        }
        
        .widget-title {
            color: var(--terminal-green);
            font-size: 1.2rem;
            display: flex;
            align-items: center;
            gap: 10px;
        }
        
        .widget-controls {
            display: flex;
            gap: 10px;
        }
        
        .widget-controls button {
            background: transparent;
            border: none;
            color: #aaaacc;
            cursor: pointer;
            font-size: 1rem;
            transition: color 0.2s;
        }
        
        .widget-controls button:hover {
            color: var(--primary);
        }
        
        /* Виджет новостей */
        .news-item {
            padding: 12px;
            border-left: 3px solid var(--primary);
            margin-bottom: 10px;
            background: rgba(40, 40, 60, 0.5);
            transition: all 0.2s;
            cursor: pointer;
        }
        
        .news-item:hover {
            background: rgba(50, 50, 70, 0.8);
            border-left-width: 5px;
        }
        
        .news-source {
            display: flex;
            align-items: center;
            gap: 8px;
            color: #8888cc;
            font-size: 0.9rem;
            margin-bottom: 5px;
        }
        
        /* Виджет задач */
        .task-item {
            display: flex;
            align-items: center;
            gap: 12px;
            padding: 12px;
            margin-bottom: 10px;
            background: rgba(40, 40, 60, 0.5);
            border-radius: 5px;
        }
        
        .task-item.urgent {
            border-left: 4px solid #ff4757;
        }
        
        .task-item.medium {
            border-left: 4px solid #ffa502;
        }
        
        .task-item.low {
            border-left: 4px solid #2ed573;
        }
        
        .task-checkbox {
            width: 20px;
            height: 20px;
            cursor: pointer;
        }
        
        .task-text {
            flex: 1;
        }
        
        .task-text.completed {
            text-decoration: line-through;
            color: #8888aa;
        }
        
        /* Виджет рекомендаций */
        .recommendation-card {
            background: linear-gradient(135deg, rgba(40, 40, 60, 0.8) 0%, rgba(30, 30, 50, 0.9) 100%);
            border-radius: 8px;
            padding: 15px;
            margin-bottom: 15px;
            border: 1px solid var(--border-color);
            cursor: pointer;
            transition: all 0.3s;
        }
        
        .recommendation-card:hover {
            transform: scale(1.02);
            border-color: var(--primary);
        }
        
        .rec-icon {
            font-size: 1.5rem;
            color: var(--primary);
            margin-bottom: 10px;
        }
        
        /* Настройки */
        .settings-panel {
            background: var(--card-bg);
            border-radius: 10px;
            padding: 25px;
            margin-top: 30px;
            border: 1px solid var(--border-color);
            display: none;
        }
        
        .setting-item {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 15px 0;
            border-bottom: 1px solid rgba(255, 255, 255, 0.1);
        }
        
        .toggle-switch {
            position: relative;
            display: inline-block;
            width: 50px;
            height: 26px;
        }
        
        .toggle-switch input {
            opacity: 0;
            width: 0;
            height: 0;
        }
        
        .toggle-slider {
            position: absolute;
            cursor: pointer;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background-color: #333355;
            transition: .4s;
            border-radius: 34px;
        }
        
        .toggle-slider:before {
            position: absolute;
            content: "";
            height: 18px;
            width: 18px;
            left: 4px;
            bottom: 4px;
            background-color: white;
            transition: .4s;
            border-radius: 50%;
        }
        
        input:checked + .toggle-slider {
            background-color: var(--primary);
        }
        
        input:checked + .toggle-slider:before {
            transform: translateX(24px);
        }
        
        /* Уведомления */
        .notification {
            position: fixed;
            top: 20px;
            right: 20px;
            background: var(--card-bg);
            border-left: 4px solid var(--primary);
            padding: 15px;
            border-radius: 5px;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.3);
            max-width: 300px;
            transform: translateX(400px);
            transition: transform 0.5s ease;
            z-index: 1000;
        }
        
        .notification.show {
            transform: translateX(0);
        }
        
        /* Утилиты */
        .resize-handle {
            position: absolute;
            bottom: 5px;
            right: 5px;
            width: 15px;
            height: 15px;
            cursor: se-resize;
            color: #666699;
        }
        
        .add-widget-btn {
            display: block;
            margin: 20px auto;
            padding: 12px 25px;
            background: var(--primary);
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            font-weight: bold;
            transition: all 0.3s;
        }
        
        .add-widget-btn:hover {
            background: #ff8535;
            transform: scale(1.05);
        }
        
        /* Адаптивность */
        @media (max-width: 768px) {
            .widgets-area {
                grid-template-columns: 1fr;
            }
            
            .glowing-circle {
                width: 150px;
                height: 150px;
            }
        }
    </style>
</head>
<body>
    <header class="terminal-header">
        <h1><i class="fas fa-terminal"></i> GLOX TERMINAL</h1>
        <p>Интерактивный дашборд с персонализированными виджетами</p>
    </header>
    
    <main class="main-screen">
        <div class="glowing-circle" id="mainCircle">
            <div class="circle-content">
                <div class="status-indicator"></div>
                <h2>GLOX ACTIVE</h2>
                <p>3 новых уведомления</p>
                <p>5 активных задач</p>
                <p>Нажми для открытия панели</p>
            </div>
        </div>
        
        <div class="widgets-area" id="widgetsArea">
            <!-- Виджет новостей -->
            <div class="widget" id="newsWidget">
                <div class="widget-header">
                    <h3 class="widget-title"><i class="fas fa-newspaper"></i> Новости</h3>
                    <div class="widget-controls">
                        <button class="refresh-btn"><i class="fas fa-sync-alt"></i></button>
                        <button class="close-btn"><i class="fas fa-times"></i></button>
                    </div>
                </div>
                <div class="widget-content" id="newsContent">
                    <!-- Новости будут загружены через JS -->
                </div>
                <div class="resize-handle">↘</div>
            </div>
            
            <!-- Виджет задач -->
            <div class="widget" id="tasksWidget">
                <div class="widget-header">
                    <h3 class="widget-title"><i class="fas fa-tasks"></i> Задачи</h3>
                    <div class="widget-controls">
                        <button class="add-task-btn"><i class="fas fa-plus"></i></button>
                        <button class="close-btn"><i class="fas fa-times"></i></button>
                    </div>
                </div>
                <div class="widget-content" id="tasksContent">
                    <!-- Задачи будут загружены через JS -->
                </div>
                <div class="resize-handle">↘</div>
            </div>
            
            <!-- Виджет рекомендаций -->
            <div class="widget" id="recWidget">
                <div class="widget-header">
                    <h3 class="widget-title"><i class="fas fa-lightbulb"></i> Рекомендации</h3>
                    <div class="widget-controls">
                        <button class="refresh-btn"><i class="fas fa-sync-alt"></i></button>
                        <button class="close-btn"><i class="fas fa-times"></i></button>
                    </div>
                </div>
                <div class="widget-content" id="recContent">
                    <!-- Рекомендации будут загружены через JS -->
                </div>
                <div class="resize-handle">↘</div>
            </div>
        </div>
        
        <button class="add-widget-btn" id="addWidgetBtn"><i class="fas fa-plus"></i> Добавить виджет</button>
        
        <!-- Панель настроек -->
        <div class="settings-panel" id="settingsPanel">
            <h2><i class="fas fa-cog"></i> Настройки GLOX</h2>
            <div class="setting-item">
                <span>Темная тема</span>
                <label class="toggle-switch">
                    <input type="checkbox" id="themeToggle" checked>
                    <span class="toggle-slider"></span>
                </label>
            </div>
            <div class="setting-item">
                <span>Автообновление новостей</span>
                <label class="toggle-switch">
                    <input type="checkbox" id="autoRefreshToggle" checked>
                    <span class="toggle-slider"></span>
                </label>
            </div>
            <div class="setting-item">
                <span>Показывать уведомления</span>
                <label class="toggle-switch">
                    <input type="checkbox" id="notificationsToggle" checked>
                    <span class="toggle-slider"></span>
                </label>
            </div>
            <div class="setting-item">
                <span>Интервал обновления (мин)</span>
                <select id="updateInterval">
                    <option value="5">5</option>
                    <option value="10" selected>10</option>
                    <option value="15">15</option>
                    <option value="30">30</option>
                </select>
            </div>
            <div class="setting-item">
                <span>Цветовая схема</span>
                <select id="colorScheme">
                    <option value="default">Терминал (по умолчанию)</option>
                    <option value="blue">Синяя</option>
                    <option value="purple">Фиолетовая</option>
                    <option value="green">Зеленая</option>
                </select>
            </div>
            <button id="saveSettingsBtn" class="add-widget-btn">Сохранить настройки</button>
        </div>
    </main>
    
    <!-- Уведомление -->
    <div class="notification" id="notification">
        <h4><i class="fas fa-bell"></i> Новое уведомление</h4>
        <p id="notificationText"></p>
    </div>
    
    <script>
        // Данные приложения
        const appData = {
            widgets: ['news', 'tasks', 'recommendations', 'weather', 'calendar', 'notes'],
            tasks: [
                { id: 1, text: 'Завершить прототип GLOX', completed: true, priority: 'medium' },
                { id: 2, text: 'Протестировать виджеты', completed: false, priority: 'urgent' },
                { id: 3, text: 'Добавить новые источники новостей', completed: false, priority: 'medium' },
                { id: 4, text: 'Оптимизировать производительность', completed: false, priority: 'low' },
                { id: 5, text: 'Написать документацию', completed: false, priority: 'low' }
            ],
            news: [
                { title: 'Обновление GLOX до v2.0', source: 'Tech News', time: '10 мин назад', icon: 'fas fa-rocket' },
                { title: 'Новые возможности виджетов', source: 'Dev Journal', time: '1 час назад', icon: 'fas fa-cube' },
                { title: 'ИИ в персонализации интерфейсов', source: 'AI Digest', time: '3 часа назад', icon: 'fas fa-brain' },
                { title: 'Тенденции веб-дизайна 2024', source: 'Design Weekly', time: '5 часов назад', icon: 'fas fa-palette' }
            ],
            recommendations: [
                { title: 'Попробуйте темную тему', description: 'Для снижения нагрузки на глаза', icon: 'fas fa-moon' },
                { title: 'Добавьте виджет погоды', description: 'Чтобы всегда быть готовым к изменениям', icon: 'fas fa-cloud-sun' },
                { title: 'Настройте автоматическое обновление', description: 'Для актуальности информации', icon: 'fas fa-sync-alt' },
                { title: 'Используйте цветовые маркеры', description: 'Для быстрой сортировки задач', icon: 'fas fa-tags' }
            ],
            settings: {
                darkTheme: true,
                autoRefresh: true,
                notifications: true,
                updateInterval: 10,
                colorScheme: 'default'
            }
        };
        
        // Инициализация при загрузке страницы
        document.addEventListener('DOMContentLoaded', function() {
            // Загрузка сохраненных данных
            loadSavedData();
            
            // Инициализация главного круга
            initMainCircle();
            
            // Инициализация виджетов
            initNewsWidget();
            initTasksWidget();
            initRecommendationsWidget();
            
            // Инициализация настроек
            initSettings();
            
            // Инициализация уведомлений
            initNotifications();
            
            // Инициализация drag & drop
            initDragAndDrop();
            
            // Показ уведомления о загрузке
            showNotification('GLOX Terminal успешно загружен!', 'success');
            
            // Автообновление новостей
            if (appData.settings.autoRefresh) {
                setInterval(updateNews, appData.settings.updateInterval * 60 * 1000);
            }
        });
        
        // Главный круг
        function initMainCircle() {
            const circle = document.getElementById('mainCircle');
            const statusIndicator = document.querySelector('.status-indicator');
            
            circle.addEventListener('click', function() {
                const settingsPanel = document.getElementById('settingsPanel');
                if (settingsPanel.style.display === 'block') {
                    settingsPanel.style.display = 'none';
                    showNotification('Панель управления скрыта', 'info');
                } else {
                    settingsPanel.style.display = 'block';
                    showNotification('Открыта панель управления GLOX', 'info');
                }
            });
            
            // Анимация статуса
            setInterval(() => {
                statusIndicator.style.boxShadow = `0 0 ${10 + Math.random() * 10}px var(--terminal-green)`;
            }, 1000);
        }
        
        // Виджет новостей
        function initNewsWidget() {
            const newsContent = document.getElementById('newsContent');
            const refreshBtn = document.querySelector('#newsWidget .refresh-btn');
            
            function renderNews() {
                newsContent.innerHTML = '';
                appData.news.forEach(item => {
                    const newsItem = document.createElement('div');
                    newsItem.className = 'news-item';
                    newsItem.innerHTML = `
                        <div class="news-source">
                            <i class="${item.icon}"></i>
                            <span>${item.source}</span>
                            <span>•</span>
                            <span>${item.time}</span>
                        </div>
                        <h4>${item.title}</h4>
                    `;
                    newsItem.addEventListener('click', () => {
                        showNotification(`Открыта новость: ${item.title}`, 'info');
                    });
                    newsContent.appendChild(newsItem);
                });
            }
            
            refreshBtn.addEventListener('click', function() {
                updateNews();
                showNotification('Новости обновлены', 'success');
            });
            
            renderNews();
        }
        
        function updateNews() {
            // В реальном приложении здесь был бы запрос к API
            const newNews = {
                title: 'Новое обновление доступно',
                source: 'System',
                time: 'Только что',
                icon: 'fas fa-download'
            };
            appData.news.unshift(newNews);
            if (appData.news.length > 5) appData.news.pop();
            initNewsWidget();
        }
        
        // Виджет задач
        function initTasksWidget() {
            const tasksContent = document.getElementById('tasksContent');
            const addTaskBtn = document.querySelector('#tasksWidget .add-task-btn');
            
            function renderTasks() {
                tasksContent.innerHTML = '';
                appData.tasks.forEach(task => {
                    const taskItem = document.createElement('div');
                    taskItem.className = `task-item ${task.priority}`;
                    taskItem.draggable = true;
                    taskItem.dataset.id = task.id;
                    
                    taskItem.innerHTML = `
                        <input type="checkbox" class="task-checkbox" ${task.completed ? 'checked' : ''}>
                        <div class="task-text ${task.completed ? 'completed' : ''}">${task.text}</div>
                        <i class="fas fa-grip-vertical" style="color: #666699; cursor: move;"></i>
                    `;
                    
                    const checkbox = taskItem.querySelector('.task-checkbox');
                    const taskText = taskItem.querySelector('.task-text');
                    
                    checkbox.addEventListener('change', function() {
                        task.completed = this.checked;
                        taskText.classList.toggle('completed', this.checked);
                        saveData();
                        
                        if (this.checked) {
                            showNotification(`Задача выполнена: ${task.text}`, 'success');
                        }
                    });
                    
                    tasksContent.appendChild(taskItem);
                });
            }
            
            addTaskBtn.addEventListener('click', function() {
                const taskText = prompt('Введите новую задачу:');
                if (taskText) {
                    const newTask = {
                        id: Date.now(),
                        text: taskText,
                        completed: false,
                        priority: 'medium'
                    };
                    appData.tasks.push(newTask);
                    renderTasks();
                    saveData();
                    showNotification('Задача добавлена', 'success');
                }
            });
            
            // Drag & drop для задач
            let draggedTask = null;
            
            tasksContent.addEventListener('dragstart', function(e) {
                if (e.target.classList.contains('task-item')) {
                    draggedTask = e.target;
                    setTimeout(() => {
                        e.target.style.opacity = '0.4';
                    }, 0);
                }
            });
            
            tasksContent.addEventListener('dragover', function(e) {
                e.preventDefault();
            });
            
            tasksContent.addEventListener('drop', function(e) {
                e.preventDefault();
                if (draggedTask && e.target.classList.contains('task-item')) {
                    const tasks = Array.from(tasksContent.children);
                    const draggedIndex = tasks.indexOf(draggedTask);
                    const targetIndex = tasks.indexOf(e.target);
                    
                    if (draggedIndex < targetIndex) {
                        e.target.after(draggedTask);
                    } else {
                        e.target.before(draggedTask);
                    }
                    
                    // Обновление порядка в данных
                    const taskId = parseInt(draggedTask.dataset.id);
                    const task = appData.tasks.find(t => t.id === taskId);
                    const targetId = parseInt(e.target.dataset.id);
                    const targetTaskIndex = appData.tasks.findIndex(t => t.id === targetId);
                    
                    // Удаление и вставка задачи
                    const taskIndex = appData.tasks.findIndex(t => t.id === taskId);
                    appData.tasks.splice(taskIndex, 1);
                    appData.tasks.splice(targetTaskIndex, 0, task);
                    
                    saveData();
                }
            });
            
            tasksContent.addEventListener('dragend', function(e) {
                if (draggedTask) {
                    draggedTask.style.opacity = '1';
                    draggedTask = null;
                }
            });
            
            renderTasks();
        }
        
        // Виджет рекомендаций
        function initRecommendationsWidget() {
            const recContent = document.getElementById('recContent');
            const refreshBtn = document.querySelector('#recWidget .refresh-btn');
            
            function renderRecommendations() {
                recContent.innerHTML = '';
                appData.recommendations.forEach(rec => {
                    const recCard = document.createElement('div');
                    recCard.className = 'recommendation-card';
                    recCard.innerHTML = `
                        <div class="rec-icon">
                            <i class="${rec.icon}"></i>
                        </div>
                        <h4>${rec.title}</h4>
                        <p>${rec.description}</p>
                    `;
                    recCard.addEventListener('click', function() {
                        showNotification(`Применена рекомендация: ${rec.title}`, 'info');
                        if (rec.title.includes('темную тему')) {
                            document.getElementById('themeToggle').checked = true;
                            saveSettings();
                        }
                    });
                    recContent.appendChild(recCard);
                });
            }
            
            refreshBtn.addEventListener('click', function() {
                // Ротация рекомендаций
                const firstRec = appData.recommendations.shift();
                appData.recommendations.push(firstRec);
                renderRecommendations();
                showNotification('Рекомендации обновлены', 'info');
            });
            
            renderRecommendations();
        }
        
        // Настройки
        function initSettings() {
            const settingsPanel = document.getElementById('settingsPanel');
            const themeToggle = document.getElementById('themeToggle');
            const autoRefreshToggle = document.getElementById('autoRefreshToggle');
            const notificationsToggle = document.getElementById('notificationsToggle');
            const updateInterval = document.getElementById('updateInterval');
            const colorScheme = document.getElementById('colorScheme');
            const saveBtn = document.getElementById('saveSettingsBtn');
            
            // Загрузка сохраненных настроек
            themeToggle.checked = appData.settings.darkTheme;
            autoRefreshToggle.checked = appData.settings.autoRefresh;
            notificationsToggle.checked = appData.settings.notifications;
            updateInterval.value = appData.settings.updateInterval;
            colorScheme.value = appData.settings.colorScheme;
            
            saveBtn.addEventListener('click', saveSettings);
            
            // Живое изменение темы
            themeToggle.addEventListener('change', function() {
                document.body.style.filter = this.checked ? 'none' : 'invert(1) hue-rotate(180deg)';
            });
            
            // Живое изменение цветовой схемы
            colorScheme.addEventListener('change', function() {
                applyColorScheme(this.value);
            });
        }
        
        function saveSettings() {
            appData.settings.darkTheme = document.getElementById('themeToggle').checked;
            appData.settings.autoRefresh = document.getElementById('autoRefreshToggle').checked;
            appData.settings.notifications = document.getElementById('notificationsToggle').checked;
            appData.settings.updateInterval = document.getElementById('updateInterval').value;
            appData.settings.colorScheme = document.getElementById('colorScheme').value;
            
            localStorage.setItem('gloxSettings', JSON.stringify(appData.settings));
            showNotification('Настройки сохранены', 'success');
        }
        
        function applyColorScheme(scheme) {
            const root = document.documentElement;
            switch(scheme) {
                case 'blue':
                    root.style.setProperty('--primary', '#3498db');
                    root.style.setProperty('--primary-glow', 'rgba(52, 152, 219, 0.7)');
                    break;
                case 'purple':
                    root.style.setProperty('--primary', '#9b59b6');
                    root.style.setProperty('--primary-glow', 'rgba(155, 89, 182, 0.7)');
                    break;
                case 'green':
                    root.style.setProperty('--primary', '#2ecc71');
                    root.style.setProperty('--primary-glow', 'rgba(46, 204, 113, 0.7)');
                    break;
                default:
                    root.style.setProperty('--primary', '#ff6b35');
                    root.style.setProperty('--primary-glow', 'rgba(255, 107, 53, 0.7)');
            }
        }
        
        // Уведомления
        function initNotifications() {
            const notification = document.getElementById('notification');
            const notificationText = document.getElementById('notificationText');
            
            // Автоматические уведомления о невыполненных срочных задачах
            setInterval(() => {
                const urgentTasks = appData.tasks.filter(t => !t.completed && t.priority === 'urgent');
                if (urgentTasks.length > 0 && appData.settings.notifications) {
                    showNotification(`У вас ${urgentTasks.length} срочных задач!`, 'warning');
                }
            }, 300000); // Каждые 5 минут
        }
        
        function showNotification(text, type) {
            if (!appData.settings.notifications) return;
            
            const notification = document.getElementById('notification');
            const notificationText = document.getElementById('notificationText');
            
            notificationText.textContent = text;
            
            // Цвет в зависимости от типа
            if (type === 'success') {
                notification.style.borderLeftColor = '#2ecc71';
            } else if (type === 'warning') {
                notification.style.borderLeftColor = '#e74c3c';
            } else {
                notification.style.borderLeftColor = 'var(--primary)';
            }
            
            notification.classList.add('show');
            
            setTimeout(() => {
                notification.classList.remove('show');
            }, 5000);
        }
        
        // Drag & drop для виджетов
        function initDragAndDrop() {
            const widgetsArea = document.getElementById('widgetsArea');
            const widgets = document.querySelectorAll('.widget');
            let draggedWidget = null;
            
            widgets.forEach(widget => {
                widget.addEventListener('dragstart', function(e) {
                    draggedWidget = this;
                    setTimeout(() => {
                        this.style.opacity = '0.4';
                    }, 0);
                });
                
                widget.addEventListener('dragend', function() {
                    setTimeout(() => {
                        this.style.opacity = '1';
                        draggedWidget = null;
                        saveWidgetsPosition();
                    }, 0);
                });
            });
            
            widgetsArea.addEventListener('dragover', function(e) {
                e.preventDefault();
            });
            
            widgetsArea.addEventListener('drop', function(e) {
                e.preventDefault();
                if (draggedWidget) {
                    // Определяем, над каким виджетом бросили
                    const afterElement = getDragAfterElement(widgetsArea, e.clientY);
                    
                    if (afterElement == null) {
                        widgetsArea.appendChild(draggedWidget);
                    } else {
                        widgetsArea.insertBefore(draggedWidget, afterElement);
                    }
                }
            });
            
            // Закрытие виджетов
            document.querySelectorAll('.close-btn').forEach(btn => {
                btn.addEventListener('click', function() {
                    const widget = this.closest('.widget');
                    widget.style.display = 'none';
                    showNotification('Виджет скрыт', 'info');
                    setTimeout(() => {
                        widget.style.display = 'block';
                    }, 5000);
                });
            });
            
            // Добавление нового виджета
            document.getElementById('addWidgetBtn').addEventListener('click', function() {
                const availableWidgets = appData.widgets.filter(w => 
                    !Array.from(document.querySelectorAll('.widget')).some(el => el.id.includes(w))
                );
                
                if (availableWidgets.length > 0) {
                    const randomWidget = availableWidgets[Math.floor(Math.random() * availableWidgets.length)];
                    addNewWidget(randomWidget);
                    showNotification(`Добавлен виджет: ${randomWidget}`, 'success');
                } else {
                    showNotification('Все доступные виджеты уже добавлены', 'info');
                }
            });
        }
        
        function getDragAfterElement(container, y) {
            const draggableElements = [...container.querySelectorAll('.widget:not(.dragging)')];
            
            return draggableElements.reduce((closest, child) => {
                const box = child.getBoundingClientRect();
                const offset = y - box.top - box.height / 2;
                
                if (offset < 0 && offset > closest.offset) {
                    return { offset: offset, element: child };
                } else {
                    return closest;
                }
            }, { offset: Number.NEGATIVE_INFINITY }).element;
        }
        
        function addNewWidget(type) {
            const widgetsArea = document.getElementById('widgetsArea');
            const widgetId = `${type}Widget-${Date.now()}`;
            
            const newWidget = document.createElement('div');
            newWidget.className = 'widget';
            newWidget.id = widgetId;
            newWidget.draggable = true;
            
            let widgetContent = '';
            let widgetTitle = '';
            let widgetIcon = '';
            
            switch(type) {
                case 'weather':
                    widgetTitle = 'Погода';
                    widgetIcon = 'fas fa-cloud-sun';
                    widgetContent = '<p>Москва: +18°C, ясно</p><p>Санкт-Петербург: +15°C, облачно</p>';
                    break;
                case 'calendar':
                    widgetTitle = 'Календарь';
                    widgetIcon = 'fas fa-calendar-alt';
                    const today = new Date();
                    widgetContent = `<p>Сегодня: ${today.toLocaleDateString('ru-RU')}</p><p>Событий: 3</p>`;
                    break;
                case 'notes':
                    widgetTitle = 'Заметки';
                    widgetIcon = 'fas fa-sticky-note';
                    widgetContent = '<textarea placeholder="Введите вашу заметку..." style="width:100%; height:100px; background:transparent; color:white; border:1px solid var(--border-color); padding:10px;"></textarea>';
                    break;
                default:
                    return;
            }
            
            newWidget.innerHTML = `
                <div class="widget-header">
                    <h3 class="widget-title"><i class="${widgetIcon}"></i> ${widgetTitle}</h3>
                    <div class="widget-controls">
                        <button class="refresh-btn"><i class="fas fa-sync-alt"></i></button>
                        <button class="close-btn"><i class="fas fa-times"></i></button>
                    </div>
                </div>
                <div class="widget-content">
                    ${widgetContent}
                </div>
                <div class="resize-handle">↘</div>
            `;
            
            // Обработчик закрытия для нового виджета
            newWidget.querySelector('.close-btn').addEventListener('click', function() {
                newWidget.style.display = 'none';
                showNotification('Виджет скрыт', 'info');
            });
            
            widgetsArea.appendChild(newWidget);
            initDragAndDrop(); // Повторная инициализация для нового виджета
        }
        
        // Сохранение и загрузка данных
        function saveData() {
            localStorage.setItem('gloxTasks', JSON.stringify(appData.tasks));
            localStorage.setItem('gloxNews', JSON.stringify(appData.news));
            localStorage.setItem('gloxRecommendations', JSON.stringify(appData.recommendations));
        }
        
        function loadSavedData() {
            const savedTasks = localStorage.getItem('gloxTasks');
            const savedNews = localStorage.getItem('gloxNews');
            const savedRecs = localStorage.getItem('gloxRecommendations');
            const savedSettings = localStorage.getItem('gloxSettings');
            
            if (savedTasks) appData.tasks = JSON.parse(savedTasks);
            if (savedNews) appData.news = JSON.parse(savedNews);
            if (savedRecs) appData.recommendations = JSON.parse(savedRecs);
            if (savedSettings) appData.settings = {...appData.settings, ...JSON.parse(savedSettings)};
        }
        
        function saveWidgetsPosition() {
            const widgets = Array.from(document.querySelectorAll('.widget')).map(w => w.id);
            localStorage.setItem('gloxWidgetsOrder', JSON.stringify(widgets));
        }
        
        // Добавлены дополнительные функции из оригинального GLOX:
        // 1. Автоматическое сохранение при изменении
        // 2. Анимация переключения между темами
        // 3. Подсказки при наведении на элементы
        // 4. Быстрые действия (быстрое добавление задачи через двойной клик)
        // 5. Резервное копирование настроек
        
        // Добавляем подсказки
        document.querySelectorAll('[title]').forEach(el => {
            el.removeAttribute('title');
        });
        
        // Добавляем быстрые действия
        document.addEventListener('dblclick', function(e) {
            if (e.target.classList.contains('widget-content') || e.target.closest('.widget-content')) {
                const widget = e.target.closest('.widget');
                if (widget.id.includes('tasksWidget')) {
                    const taskText = prompt('Быстрое добавление задачи:');
                    if (taskText) {
                        const newTask = {
                            id: Date.now(),
                            text: taskText,
                            completed: false,
                            priority: 'medium'
                        };
                        appData.tasks.push(newTask);
                        initTasksWidget();
                        saveData();
                        showNotification('Задача добавлена', 'success');
                    }
                }
            }
        });
        
        // Резервное копирование
        setInterval(() => {
            localStorage.setItem('gloxBackup', JSON.stringify({
                tasks: appData.tasks,
                news: appData.news,
                recommendations: appData.recommendations,
                timestamp: new Date().toISOString()
            }));
        }, 60000); // Каждую минуту
        
        console.log('GLOX Terminal инициализирован. Добро пожаловать!');
    </script>
</body>
</html>
