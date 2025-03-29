document.addEventListener('DOMContentLoaded', function() {
    // Timer state
    let timerSettings = {
        'Pomodoro': 25,
        'Short Break': 5,
        'Long Break': 15
    };
    
    let currentTimerType = 'Pomodoro';
    let minutes = timerSettings[currentTimerType];
    let seconds = 0;
    let isActive = false;
    let timerInterval;
    let tasks = JSON.parse(localStorage.getItem('tasks')) || [];
    let selectedTaskId = null;
    
    // DOM elements
    const timerEl = document.getElementById('timer');
    const startButton = document.getElementById('start-button');
    const tabButtons = document.querySelectorAll('.tab-button');
    const settingsBtn = document.getElementById('settings-btn');
    const menuBtn = document.getElementById('menu-btn');
    const taskMenuBtn = document.getElementById('task-menu-btn');
    const addTaskButton = document.getElementById('add-task-button');
    const addTaskForm = document.getElementById('add-task-form');
    const newTaskInput = document.getElementById('new-task-input');
    const saveButton = document.getElementById('save-button');
    const cancelButton = document.getElementById('cancel-button');
    const tasksListEl = document.getElementById('tasks-list');
    
    // Modals
    const settingsModal = document.getElementById('settings-modal');
    const closeSettings = document.getElementById('close-settings');
    const saveSettings = document.getElementById('save-settings');
    const cancelSettings = document.getElementById('cancel-settings');
    const pomodoroTimeInput = document.getElementById('pomodoro-time');
    const shortBreakTimeInput = document.getElementById('short-break-time');
    const longBreakTimeInput = document.getElementById('long-break-time');
    
    const menuModal = document.getElementById('menu-modal');
    const resetTimer = document.getElementById('reset-timer');
    const openSettings = document.getElementById('open-settings');
    
    const taskMenuModal = document.getElementById('task-menu-modal');
    const deleteAllTasks = document.getElementById('delete-all-tasks');
    const closeTaskMenu = document.getElementById('close-task-menu');
    
    const taskItemMenuModal = document.getElementById('task-item-menu-modal');
    const deleteTask = document.getElementById('delete-task');
    const closeTaskItemMenu = document.getElementById('close-task-item-menu');
    
    // Initialize timer display
    updateTimerDisplay();
    renderTasks();
    
    // Event listeners for tab buttons
    tabButtons.forEach(button => {
        button.addEventListener('click', function() {
            currentTimerType = this.dataset.type;
            // Update active tab
            tabButtons.forEach(btn => btn.classList.remove('active'));
            this.classList.add('active');
            
            // Reset timer
            resetTimerState();
        });
    });
    
    // Start/Pause button
    startButton.addEventListener('click', function() {
        toggleTimer();
    });
    
    // Settings button
    settingsBtn.addEventListener('click', function() {
        openSettingsModal();
    });
    
    // Menu button
    menuBtn.addEventListener('click', function() {
        toggleMenuModal();
    });
    
    // Task menu button
    taskMenuBtn.addEventListener('click', function() {
        toggleTaskMenuModal();
    });
    
    // Add task button
    addTaskButton.addEventListener('click', function() {
        showAddTaskForm();
    });
    
    // Save task button
    saveButton.addEventListener('click', function() {
        saveTask();
    });
    
    // Cancel add task
    cancelButton.addEventListener('click', function() {
        hideAddTaskForm();
    });
    
    // Enter key in task input
    newTaskInput.addEventListener('keypress', function(e) {
        if (e.key === 'Enter') {
            saveTask();
        }
    });
    
    // Close settings
    closeSettings.addEventListener('click', function() {
        settingsModal.style.display = 'none';
    });
    
    // Save settings
    saveSettings.addEventListener('click', function() {
        saveTimerSettings();
    });
    
    // Cancel settings
    cancelSettings.addEventListener('click', function() {
        settingsModal.style.display = 'none';
    });
    
    // Reset timer from menu
    resetTimer.addEventListener('click', function() {
        resetTimerState();
        menuModal.style.display = 'none';
    });
    
    // Open settings from menu
    openSettings.addEventListener('click', function() {
        menuModal.style.display = 'none';
        openSettingsModal();
    });
    
    // Delete all tasks
    deleteAllTasks.addEventListener('click', function() {
        tasks = [];
        saveTasks();
        renderTasks();
        taskMenuModal.style.display = 'none';
    });
    
    // Close task menu
    closeTaskMenu.addEventListener('click', function() {
        taskMenuModal.style.display = 'none';
    });
    
    // Delete task
    deleteTask.addEventListener('click', function() {
        if (selectedTaskId !== null) {
            tasks = tasks.filter(task => task.id !== selectedTaskId);
            saveTasks();
            renderTasks();
            taskItemMenuModal.style.display = 'none';
        }
    });
    
    // Close task item menu
    closeTaskItemMenu.addEventListener('click', function() {
        taskItemMenuModal.style.display = 'none';
    });
    
    // Click outside modal to close
    window.addEventListener('click', function(e) {
        if (e.target === settingsModal) {
            settingsModal.style.display = 'none';
        }
        if (e.target !== menuBtn && menuModal.style.display === 'block') {
            if (!menuModal.contains(e.target)) {
                menuModal.style.display = 'none';
            }
        }
        if (e.target !== taskMenuBtn && taskMenuModal.style.display === 'block') {
            if (!taskMenuModal.contains(e.target)) {
                taskMenuModal.style.display = 'none';
            }
        }
        if (taskItemMenuModal.style.display === 'block') {
            if (!taskItemMenuModal.contains(e.target)) {
                taskItemMenuModal.style.display = 'none';
            }
        }
    });
    
    // Timer functions
    function toggleTimer() {
        isActive = !isActive;
        
        if (isActive) {
            startButton.textContent = 'PAUSE';
            startTimer();
        } else {
            startButton.textContent = 'START';
            clearInterval(timerInterval);
        }
    }
    
    function startTimer() {
        timerInterval = setInterval(function() {
            if (seconds === 0) {
                if (minutes === 0) {
                    // Timer completed
                    clearInterval(timerInterval);
                    isActive = false;
                    startButton.textContent = 'START';
                    
                    // Alert user
                    alert(`${currentTimerType} completed!`);
                    
                    return;
                }
                
                minutes--;
                seconds = 59;
            } else {
                seconds--;
            }
            
            updateTimerDisplay();
        }, 1000);
    }
    
    function updateTimerDisplay() {
        timerEl.textContent = `${minutes < 10 ? '0' : ''}${minutes}:${seconds < 10 ? '0' : ''}${seconds}`;
    }
    
    function resetTimerState() {
        clearInterval(timerInterval);
        isActive = false;
        minutes = timerSettings[currentTimerType];
        seconds = 0;
        updateTimerDisplay();
        startButton.textContent = 'START';
    }
    
    // Settings functions
    function openSettingsModal() {
        // Set current values
        pomodoroTimeInput.value = timerSettings['Pomodoro'];
        shortBreakTimeInput.value = timerSettings['Short Break'];
        longBreakTimeInput.value = timerSettings['Long Break'];
        
        settingsModal.style.display = 'flex';
    }
    
    function saveTimerSettings() {
        // Validate inputs
        const pomodoro = parseInt(pomodoroTimeInput.value) || 25;
        const shortBreak = parseInt(shortBreakTimeInput.value) || 5;
        const longBreak = parseInt(longBreakTimeInput.value) || 15;
        
        timerSettings = {
            'Pomodoro': Math.min(Math.max(1, pomodoro), 60),
            'Short Break': Math.min(Math.max(1, shortBreak), 30),
            'Long Break': Math.min(Math.max(1, longBreak), 60)
        };
        
        // Save to local storage
        localStorage.setItem('timerSettings', JSON.stringify(timerSettings));
        
        // Reset current timer
        resetTimerState();
        
        // Close modal
        settingsModal.style.display = 'none';
    }
    
    // Menu functions
    function toggleMenuModal() {
        if (menuModal.style.display === 'block') {
            menuModal.style.display = 'none';
        } else {
            menuModal.style.display = 'block';
            taskMenuModal.style.display = 'none';
            taskItemMenuModal.style.display = 'none';
        }
    }
    
    function toggleTaskMenuModal() {
        if (taskMenuModal.style.display === 'block') {
            taskMenuModal.style.display = 'none';
        } else {
            taskMenuModal.style.display = 'block';
            menuModal.style.display = 'none';
            taskItemMenuModal.style.display = 'none';
        }
    }
    
    // Task functions
    function showAddTaskForm() {
        addTaskButton.style.display = 'none';
        addTaskForm.style.display = 'block';
        newTaskInput.focus();
    }
    
    function hideAddTaskForm() {
        addTaskButton.style.display = 'flex';
        addTaskForm.style.display = 'none';
        newTaskInput.value = '';
    }
    
    function saveTask() {
        const taskText = newTaskInput.value.trim();
        
        if (taskText) {
            const newTask = {
                id: Date.now(),
                text: taskText,
                completed: false
            };
            
            tasks.push(newTask);
            saveTasks();
            renderTasks();
            hideAddTaskForm();
        }
    }
    
    function saveTasks() {
        localStorage.setItem('tasks', JSON.stringify(tasks));
    }
    
    function renderTasks() {
        tasksListEl.innerHTML = '';
        
        tasks.forEach(task => {
            const taskEl = document.createElement('div');
            taskEl.className = 'task-item';
            
            const taskText = document.createElement('div');
            taskText.textContent = task.text;
            
            const taskMenu = document.createElement('button');
            taskMenu.className = 'icon-button';
            taskMenu.innerHTML = '⋮';
            taskMenu.addEventListener('click', function(e) {
                e.stopPropagation();
                selectedTaskId = task.id;
                
                // Position the menu near the clicked button
                const rect = this.getBoundingClientRect();
                taskItemMenuModal.style.display = 'block';
                taskItemMenuModal.style.top = `${rect.bottom}px`;
                taskItemMenuModal.style.left = `${rect.left - 150}px`; // Adjust as needed
                
                menuModal.style.display = 'none';
                taskMenuModal.style.display = 'none';
            });
            
            taskEl.appendChild(taskText);
            taskEl.appendChild(taskMenu);
            tasksListEl.appendChild(taskEl);
        });
    }
    
    // Load saved settings
    const savedSettings = localStorage.getItem('timerSettings');
    if (savedSettings) {
        timerSettings = JSON.parse(savedSettings);
        resetTimerState();
    }
});
