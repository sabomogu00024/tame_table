<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>タスク管理 タイムテーブル＆ToDoリスト</title>
    
    <style>
        /* --- 全体設定 --- */
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: #f0f4f8; /* 薄いグレーブルーの背景 */
            color: #333;
            line-height: 1.6;
            padding: 30px;
        }

        .container {
            max-width: 900px;
            margin: 0 auto;
            background: #ffffff;
            padding: 30px;
            border-radius: 8px;
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.1);
        }

        h1 {
            text-align: center;
            color: #1a73e8; /* 鮮やかな青 */
            margin-bottom: 30px;
            font-size: 2em;
            border-bottom: 2px solid #e0e0e0;
            padding-bottom: 10px;
        }

        h2 {
            color: #3f51b5; /* 落ち着いたインディゴブルー */
            border-bottom: 2px solid #bbdefb;
            padding-bottom: 5px;
            margin-bottom: 15px;
            font-size: 1.5em;
        }

        /* --- 入力・設定エリア --- */
        .date-section, .input-section, .settings {
            background: #f7f9fc;
            padding: 15px;
            border-radius: 6px;
            margin-bottom: 20px;
            display: flex;
            align-items: center;
            gap: 15px;
            border: 1px solid #e3f2fd;
            flex-wrap: wrap;
        }

        label {
            font-weight: 600;
            color: #1e88e5;
            white-space: nowrap;
        }

        input[type="text"], input[type="number"], input[type="time"], input[type="date"] {
            padding: 10px 12px;
            border: 1px solid #ccc;
            border-radius: 4px;
            flex-grow: 1;
            transition: border-color 0.3s, box-shadow 0.3s;
            min-width: 80px;
        }

        input:focus {
            border-color: #1a73e8;
            box-shadow: 0 0 0 3px rgba(26, 115, 232, 0.2);
            outline: none;
        }

        /* 日付セクションの調整 */
        .date-section {
            justify-content: space-between;
        }
        .today-date-display {
            font-weight: 600;
            color: #0b519c;
        }

        /* --- ボタンデザイン --- */
        button {
            background-color: #1a73e8;
            color: white;
            padding: 10px 15px;
            border: none;
            border-radius: 4px;
            cursor: pointer;
            font-weight: 500;
            transition: background-color 0.2s, transform 0.1s;
        }

        button:hover {
            background-color: #0b519c;
            transform: translateY(-1px);
        }

        .save-button {
            background-color: #4CAF50;
        }
        .save-button:hover {
            background-color: #388E3C;
        }
        .load-button {
            background-color: #FFC107;
            color: #333;
        }
        .load-button:hover {
            background-color: #FFB300;
        }

        /* --- タイムテーブル --- */
        .timetable-container {
            margin-top: 25px;
        }

        #timetable {
            width: 100%;
            border-collapse: collapse;
            margin-top: 15px;
            border: 1px solid #e0e0e0;
            border-radius: 4px;
            overflow: hidden;
        }

        #timetable th, #timetable td {
            border: 1px solid #f0f0f0;
            padding: 12px;
            text-align: left;
        }

        #timetable thead th {
            background-color: #e3f2fd;
            color: #1a73e8;
            font-weight: 700;
        }

        #timetable tbody tr:nth-child(even) {
            background-color: #f9f9f9;
        }

        #timetable tbody tr:hover {
            background-color: #eef5fc;
        }

        /* --- 前日のスケジュールエリア --- */
        .yesterday-schedule {
            margin-top: 25px;
            padding: 15px;
            border: 1px solid #bbdefb;
            background-color: #e3f2fd;
            border-radius: 6px;
        }

        #yesterday-tasks {
            white-space: pre-wrap;
            padding: 10px 0;
            font-family: 'Consolas', 'Courier New', monospace;
            color: #333;
            font-size: 0.9em;
        }
        
        /* --- ToDoリストエリアのスタイル --- */
        .todo-container {
            margin-top: 30px;
            padding: 20px;
            background: #ffffff;
            border: 1px solid #e0e0e0;
            border-radius: 6px;
            box-shadow: 0 2px 8px rgba(0, 0, 0, 0.05);
        }

        .todo-container h2 {
            color: #3f51b5;
            border-bottom: 2px solid #e0e0e0;
        }

        .todo-input {
            display: flex;
            margin-bottom: 15px;
            gap: 10px;
        }

        .todo-input input {
            flex-grow: 1;
            border: 1px solid #ccc;
        }

        .todo-input button {
            background-color: #607d8b;
            width: 80px;
            padding: 8px 10px;
        }
        .todo-input button:hover {
            background-color: #455a64;
        }

        #todo-list {
            list-style: none;
            padding: 0;
        }

        #todo-list li {
            display: flex;
            align-items: center;
            justify-content: space-between;
            padding: 10px 0;
            border-bottom: 1px dotted #e0e0e0;
        }

        #todo-list li:last-child {
            border-bottom: none;
        }

        .todo-item-text {
            flex-grow: 1;
            margin-left: 10px;
            transition: color 0.3s;
        }

        .completed .todo-item-text {
            text-decoration: line-through;
            color: #9e9e9e;
        }

        .delete-todo-button {
            background: none;
            border: none;
            color: #f44336;
            cursor: pointer;
            font-weight: bold;
            padding: 5px;
            transition: color 0.2s;
        }

        .delete-todo-button:hover {
            color: #d32f2f;
            transform: none;
        }
        
        .note {
            text-align: right;
            font-size: 0.85em;
            color: #616161;
            margin-top: 10px;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>タスク管理 タイムテーブル</h1>

        <div class="date-section">
            <label for="schedule-date">スケジュール日付:</label>
            <input type="date" id="schedule-date">
            <span id="today-date-display" class="today-date-display"></span>
        </div>

        <div class="input-section">
            <label for="task-name">タスク名:</label>
            <input type="text" id="task-name" placeholder="例: 資格の勉強">

            <label for="task-start-time">開始時刻:</label>
            <input type="time" id="task-start-time" value="09:00"> 

            <label for="task-duration">所要時間 (分):</label>
            <input type="number" id="task-duration" min="5" value="60">

            <button onclick="addTask()">タスクを追加</button>
        </div>

        <div class="settings">
            <button onclick="saveSchedule()" class="save-button">この日のスケジュールを保存</button>
            <button onclick="loadSchedule()" class="load-button">保存したスケジュールを読み込む</button>
        </div>

        <div class="yesterday-schedule">
            <h2>前日（<span id="yesterday-date"></span>）の計画</h2>
            <div id="yesterday-tasks">前日のスケジュールは保存されていません。</div>
        </div>
        
        <div class="timetable-container">
            <h2>本日のタイムライン</h2>
            <table id="timetable">
                <thead>
                    <tr>
                        <th>開始時刻</th>
                        <th>終了時刻</th>
                        <th>タスク名</th>
                    </tr>
                </thead>
                <tbody>
                    </tbody>
            </table>
        </div>
        
        <div class="todo-container">
            <h2>今日のToDoリスト</h2>
            <div class="todo-input">
                <input type="text" id="new-todo" placeholder="新しいToDoを入力...">
                <button onclick="addTodo()">追加</button>
            </div>
            <ul id="todo-list">
                </ul>
        </div>
        
        <p class="note">※タスクは設定した開始時刻順に並べ替えられます。</p>
    </div>

    <script>
        // タスクとToDoを保持する配列
        let tasks = [];
        let todos = [];
        const dateInput = document.getElementById('schedule-date');

        /**
         * ページ読み込み時の初期化
         */
        document.addEventListener('DOMContentLoaded', () => {
            // 今日の日付をセット
            const today = new Date();
            const todayISO = today.toISOString().split('T')[0];
            dateInput.value = todayISO;
            document.getElementById('today-date-display').textContent = `今日は ${formatDate(today)} です`;

            // スケジュール、前日スケジュール、ToDoリストのロード
            loadSchedule(todayISO);
            loadYesterdaySchedule(today);
            loadTodos(); 
        });


        // --- タイムテーブル機能 ---

        /**
         * タスクを追加し、タイムテーブルを更新する
         */
        function addTask() {
            const taskNameInput = document.getElementById('task-name');
            const taskStartTimeInput = document.getElementById('task-start-time'); 
            const taskDurationInput = document.getElementById('task-duration');

            const name = taskNameInput.value.trim();
            const startTime = taskStartTimeInput.value; 
            const duration = parseInt(taskDurationInput.value);

            if (name === '' || startTime === '' || isNaN(duration) || duration <= 0) {
                alert('タスク名、開始時刻、正の所要時間（分）を入力してください。');
                return;
            }

            tasks.push({ name, startTime, duration }); 
            taskNameInput.value = '';
            taskStartTimeInput.value = startTime; 

            renderTimetable();
        }

        /**
         * タスクを開始時刻順に並び替え、タイムテーブルを生成する
         */
        function renderTimetable() {
            const timetableBody = document.querySelector('#timetable tbody');
            
            const sortedTasks = [...tasks].sort((a, b) => a.startTime.localeCompare(b.startTime));

            timetableBody.innerHTML = '';

            sortedTasks.forEach(task => {
                const startTimeStr = task.startTime;
                
                const [startHour, startMinute] = startTimeStr.split(':').map(Number);
                const startMinutes = startHour * 60 + startMinute;
                
                const endMinutes = startMinutes + task.duration;
                const endTimeStr = formatMinutesToTime(endMinutes);

                const row = timetableBody.insertRow();
                row.insertCell(0).textContent = startTimeStr; 
                row.insertCell(1).textContent = endTimeStr;
                row.insertCell(2).textContent = task.name;
            });
        }
        
        // --- ToDoリスト機能 ---

        /**
         * ToDoリストをlocalStorageからロードする
         */
        function loadTodos() {
            const dateKey = dateInput.value;
            const key = 'daily_todos_' + dateKey;
            const savedTodosJson = localStorage.getItem(key);

            if (savedTodosJson) {
                todos = JSON.parse(savedTodosJson);
            } else {
                todos = [];
            }
            renderTodos();
        }

        /**
         * ToDoリストをlocalStorageに保存する
         */
        function saveTodos() {
            const dateKey = dateInput.value;
            const key = 'daily_todos_' + dateKey;
            localStorage.setItem(key, JSON.stringify(todos));
        }

        /**
         * ToDoリストにアイテムを追加する
         */
        function addTodo() {
            const newTodoInput = document.getElementById('new-todo');
            const text = newTodoInput.value.trim();

            if (text === '') {
                alert('ToDoを入力してください。');
                return;
            }

            todos.push({ text: text, completed: false });
            newTodoInput.value = ''; 
            saveTodos();
            renderTodos();
        }

        /**
         * ToDoリストの表示を更新する
         */
        function renderTodos() {
            const todoListUl = document.getElementById('todo-list');
            todoListUl.innerHTML = '';

            todos.forEach((todo, index) => {
                const li = document.createElement('li');
                
                const checkbox = document.createElement('input');
                checkbox.type = 'checkbox';
                checkbox.checked = todo.completed;
                checkbox.onclick = () => toggleTodo(index);

                const span = document.createElement('span');
                span.textContent = todo.text;
                span.classList.add('todo-item-text');

                const deleteButton = document.createElement('button');
                deleteButton.textContent = 'X';
                deleteButton.classList.add('delete-todo-button');
                deleteButton.onclick = () => deleteTodo(index);

                li.appendChild(checkbox);
                li.appendChild(span);
                li.appendChild(deleteButton);
                
                if (todo.completed) {
                    li.classList.add('completed');
                }

                todoListUl.appendChild(li);
            });
        }

        /**
         * ToDoの完了状態を切り替える
         */
        function toggleTodo(index) {
            todos[index].completed = !todos[index].completed;
            saveTodos();
            renderTodos();
        }

        /**
         * ToDoを削除する
         */
        function deleteTodo(index) {
            todos.splice(index, 1);
            saveTodos();
            renderTodos();
        }


        // --- スケジュール保存/読み込み機能 ---

        /**
         * 現在のタスクリストをlocalStorageに保存する
         */
        function saveSchedule() {
            const dateKey = dateInput.value;
            if (!dateKey) {
                alert('日付を選択してください。');
                return;
            }
            localStorage.setItem(dateKey, JSON.stringify(tasks));
            saveTodos(); // ★スケジュール保存時にToDoも保存
            alert(`${dateKey} のスケジュールとToDoリストを保存しました！`);
        }

        /**
         * 選択された日付のスケジュールをlocalStorageから読み込む
         */
        function loadSchedule(dateKey = dateInput.value) {
            if (!dateKey) {
                tasks = []; 
                renderTimetable();
                return;
            }
            
            const savedTasksJson = localStorage.getItem(dateKey);

            if (savedTasksJson) {
                tasks = JSON.parse(savedTasksJson);
            } else {
                tasks = []; 
            }
            renderTimetable();
        }


        // --- 前日のスケジュール表示機能 ---

        /**
         * 指定された日付の前日のスケジュールを読み込んで表示する
         */
        function loadYesterdaySchedule(currentDate) {
            const yesterday = new Date(currentDate);
            yesterday.setDate(yesterday.getDate() - 1);
            
            const yesterdayISO = yesterday.toISOString().split('T')[0];
            const savedTasksJson = localStorage.getItem(yesterdayISO);
            const yesterdayTasksDiv = document.getElementById('yesterday-tasks');
            
            document.getElementById('yesterday-date').textContent = formatDate(yesterday);

            if (savedTasksJson) {
                const yesterdayTasks = JSON.parse(savedTasksJson);
                let scheduleText = '';

                const sortedTasks = yesterdayTasks.sort((a, b) => a.startTime.localeCompare(b.startTime));

                sortedTasks.forEach(task => {
                    const startTimeStr = task.startTime;
                    
                    const [startHour, startMinute] = startTimeStr.split(':').map(Number);
                    const startMinutes = startHour * 60 + startMinute;
                    const endMinutes = startMinutes + task.duration;
                    const endTimeStr = formatMinutesToTime(endMinutes);
                    
                    scheduleText += `${startTimeStr} - ${endTimeStr}: ${task.name}\n`;
                });

                yesterdayTasksDiv.textContent = scheduleText;
            } else {
                yesterdayTasksDiv.textContent = '前日のスケジュールは保存されていません。';
            }
        }


        // 日付が変わったときにスケジュールとToDoリストを読み込むイベントリスナー
        dateInput.addEventListener('change', () => {
            loadSchedule();
            loadTodos();
            const selectedDate = new Date(dateInput.value);
            loadYesterdaySchedule(selectedDate);
        });


        // --- ヘルパー関数 ---

        /**
         * 分をHH:MM形式の文字列にフォーマットするヘルパー関数
         */
        function formatMinutesToTime(totalMinutes) {
            const hours = Math.floor(totalMinutes / 60) % 24; 
            const minutes = totalMinutes % 60;
            
            const hoursStr = String(hours).padStart(2, '0');
            const minutesStr = String(minutes).padStart(2, '0');
            
            return `${hoursStr}:${minutesStr}`;
        }

        /**
         * Dateオブジェクトを「Y年M月D日」形式の文字列にフォーマットするヘルパー関数
         */
        function formatDate(date) {
            const year = date.getFullYear();
            const month = date.getMonth() + 1;
            const day = date.getDate();
            return `${year}年${month}月${day}日`;
        }
    </script>
</body>
</html>
