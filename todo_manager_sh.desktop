#!/bin/bash

# Terminal-Based To-Do List Manager
TODO_FILE="$HOME/Desktop/game/OS_Project/todo.csv"


if [ ! -f "$TODO_FILE" ]; then
    echo "Task,Deadline,Priority" > "$TODO_FILE"  # Initialize with a header
fi


function add_task() {
       read -p "Enter task: " task
              read -p "Enter deadline (YYYY-MM-DD HH:MM, optional): " deadline
         read -p "Enter priority (1=High, 2=Medium, 3=Low): " priority

  
  if ! [[ "$priority" =~ ^[1-3]$ ]]; then
        echo "Invalid priority. Setting default to 3 (Low)."
        priority=3
             fi

    echo "$task,$deadline,$priority" >> "$TODO_FILE"
    echo "Task added successfully."

                if [ -n "$deadline" ]; then
       
        reminder_time=$(date -d "$deadline" +"%Y%m%d%H%M")
        if [ $? -eq 0 ]; then
            echo "notify-send 'Reminder: $task'" | at -t "$reminder_time" 2>/dev/null
            echo "Reminder scheduled for $deadline."
                 else
            echo "Invalid deadline format. Reminder not scheduled."
        fi
           fi

   
    notify-send "To-Do Manager" "Task '$task' added!"
}


function view_tasks() {
           echo -e "\n--- To-Do List ---"
    column -t -s, "$TODO_FILE" | tail -n +2  # Display tasks, skip header row
          echo
}


function delete_task() {
          view_tasks
    read -p "Enter the task number to delete (starting from 1): " task_num

   
    total_tasks=$(tail -n +2 "$TODO_FILE" | wc -l) 
    if ! [[ "$task_num" =~ ^[0-9]+$ ]] || [ "$task_num" -lt 1 ] || [ "$task_num" -gt "$total_tasks" ]; then
        echo "Invalid task number."
        return
    fi


    sed -i "$((task_num + 1))d" "$TODO_FILE" 
        echo "Task #$task_num deleted successfully."

    # Notify the user
    notify-send "To-Do Manager" "Task #$task_num deleted!"
}


function sort_tasks() {
       echo "1. Sort by Priority"
        echo "2. Sort by Deadline"
    read -p "Choose sorting option: " sort_option

    case $sort_option in
        1) sort -t, -k3 -n "$TODO_FILE" -o "$TODO_FILE"; echo "Tasks sorted by priority." ;;
           2) sort -t, -k2 "$TODO_FILE" -o "$TODO_FILE"; echo "Tasks sorted by deadline." ;;
         *) echo "Invalid option." ;;
    esac
}


function main_menu() {
    while true; do
        echo -e "\n--- To-Do List Manager ---"
           echo "1. Add Task"
          echo "2. View Tasks"
        echo "3. Delete Task"
         echo "4. Sort Tasks"
        echo "5. Exit"
      read -p "Choose an option: " choice

        case $choice in
            1) add_task ;;
                2) view_tasks ;;
            3) delete_task ;;
                4) sort_tasks ;;
                     5) echo "Goodbye!"; exit ;;
            *) echo "Invalid option. Please try again." ;;
        esac
    done
}

# Run the main menu
main_menu
