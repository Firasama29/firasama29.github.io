---
title: "Building a Simple CLI Task Tracker in Python"
date: 2026-07-15
categories: [python]
tags: [python]
---

Although my expertise is mostly in Java, I've worked with Python for AWS lambda development and I have spent some time learning the fundamentals and building some utility scripts for practice.

But since I do not really practice these days, I tent to forget a lot of things, which is expected because learning is most effective when repeated.

Last week I came across a project idea from roadmap.sh to build a task tracker app in the language of choice for practice. The idea is to practice with the fundamental language features rather than relying on external libraries.

I decided to work on this project idea and chose to build one version of it in Python and another with Java. Building both versions is a good way to sharpen the basic logic, especially for a project like this, as it involves activities such as:

	- handling user input
	- working with the filesystem (reading, writing)
	- working with JSON format (storing to JSON file and fetching from it)
	- handling errors gracefully
	
The point of this article is to document the development process and share what I learned from it.

## About the task tracker CLI
The project is a command-line interface (CLI) used for tracking and managing tasks. Here are the activities it should be able to do:

- Add, update and delete tasks
- List all tasks 
- Mark tasks as done
- Mark tasks as in-progress
- List all incomplete tasks (pending, in progress)
- List all completed tasks

Let's look at each one:
### Adding a new task
To accomplish this functionality, I followed these steps:
- Before adding a new entry, I would first need to check if the file actually exists. If not, it is created.
This is handled inside `add_task()` function:

```python
	def add_task(task):
		task_id = 1
		tasks = get_tasks()
		if tasks:
			task_id = max((t["id"] for t in tasks), default=0) + 1
			add_new_task(tasks, task, task_id)
		else:
			tasks = []
			add_new_task(tasks, task, task_id)
```

The file creation is managed using `json` module, which is a handy library for working with json format and it's pretty easy to use.

The `add_task()` takes a `task` argument that represents the user input. 

First, it checks for an existing list of tasks and return empty list if none exists. For that, it invokes `get_tasks()` function which I will discuss later. When it finds existing entries in the JSON file, it calculates the highest existing task id and increments it by one: `(max(id) + 1)` because the tasks are sorted by default in insertion order. If no tasks are present in the file, a new task is simply added into an empty list and written to the file.

In both conditions, `add_new_task()` function is invoked, which takes three arguments:
- tasks list (either empty or contains existing tasks).
- the new task string to be added.
- the task id - in case of existing list, the id will be calculated based on the id of the last task plus 1, otherwise, the default id will be 1.

```python
	def add_new_task(tasks, task, task_id):
	    with open(file_name, 'w') as file:
			print(f'tasks2: {tasks}')
			entry = {"id": task_id, "task": task, "status": 'pending'}
			tasks.append(entry)
			print(f'new list: {tasks}')
			json.dump(tasks, file, indent=2)
			print(f'task added: {entry}')
```
This function defines the actual format of the tasks in JSON. First, it uses `with open(file_name, 'w') as file` to write to the file. Inside it, the task object is constructed, containing the following keys:
- id
- task
- status

The task object is then appended to tasks list.

Lastly, the list is _dumped_ to the JSON file via `json.dump()` function that takes tasks list, the file to be written to and indent (for JSON structuring) as arguments. The print() statements are for logging purpose.

Here's a sample entry in the list:
```json
	[
		{
			"id": 1,
			"task": "first task",
			"status": "pending"
		}
	]
```

### retrieving tasks
`get_tasks()` function utilizes `open()` function to access the JSON file and reads the JSON data from it using `json.load(file)` and convert it into a Python object (in this case a `list`).

```python
	def get_tasks():
		try:
			with open(file_name) as file:
				return json.load(file)
		except FileNotFoundError:
			return []
		except json.JSONDecodeError:
			raise TaskDataCorruptedError("content.json is corrupted")
```

This function handles two possible errors by catching two exceptions - missing file and invalid file content:
- `FileNotFoundError` handles the case where no file is found or the path is wrong and returns an empty list.
- `json.JSONDecodeError` handles the case where the JSON file contains invalid JSON (broken syntax).

### Updating a task
To update an entry, a user needs to view the list first, select the id of the task they would like to modify, then enter the new task:

```python
	def update_task(task_id, new_task):
		tasks = get_tasks()
		for task in tasks:
			if task_id == int(task["id"]):
				print(f"task: {task}")
				task["task"] = new_task
				update_task_list(tasks)
				print(f'updated task: {task}')
				break
```
The logic involves iterating through the list of tasks - which it gets from calling `get_tasks()`, looking at the id of each entry to find a match for `task_id` the user selected, and updating the matching entry with the user input.

The below function is what handles the actual data update:

```python
	def update_task_list(tasks):
	    with open(file_name, 'w') as file:
			json.dump(tasks, file, indent=2)
```
`with open(file_name, 'w')` is used here again as we did in `add_new_task()` to write the updated data to the file. The `w` simply means the target file is opened in **write mode**. If the file already exists, its contents are overwritten with the new data, otherwise a new file is created. `json.dump(tasks, file, indent=2)` is the function that basically converts the updated data from Python object to JSON format and writes it to the file.

## Deleting a task
Similar to updating a task, a user should be able to view the current tasks, select the id of the one they want to remove.

```python
	def delete_task(task_id):
		tasks = get_tasks()
		print(f'tasks: {tasks}')
		for task in tasks:
			if task_id == int(task["id"]):
				tasks.remove(task)
				print(f'removed task: {task}')
				update_task_list(tasks)
		print(f'tasks: {tasks}')
```
The removal mechanism relies on two steps:
- removing an item from the list with `list.remove(task)`
- updating the JSON file with the updated list, which will now be missing an entry that the user requested to delete. This step utilizes the same `update_task_list(tasks)` used earlier.

## Marking a task as in progress or done
When a new task is added to the backlog, the status is set to `pending` by default, so the user can later choose which task to work on and mark it as `in-progress`, or as `done` once it's completed. This is achieved by applying similar steps to what was done before: 
- updating a property value in an object from the list. In `update_task()` I updated the task property with a new value. Now the status needs to be updated.
- writing the updated list to the JSON file.

### In-progress
```python
	def mark_in_progress(task_id):
		tasks = get_tasks()
		for task in tasks:
			if task_id == int(task["id"]):
				task['status'] = 'in-progress'
				update_task_list(tasks)
				print(f'updated task: {task}')
```

### Done

```python
	def mark_completed(task_id):
		tasks = get_tasks()
		for task in tasks:
			if task_id == int(task["id"]):
				task['status'] = 'done'
				update_task_list(tasks)
				print(f'updated task: {task}')
```
Each function follows the same logic, including calling `update_task_list(tasks)`

## Filtering Incomplete Tasks
What if a user only wants to view pending or in-progress tasks to get an idea of their current progress?
This is done in one line with a combination of *list comprehension* and conditional filtering based on the status.
list comprehension is a shorter way to loop through a list than a normal `for` loop:

```python
	[new_value for item in collection if condition]
```

Let's write the function in each version:

### Normal `for` loop:
```python
	def filter_incomplete_tasks():
		tasks = get_tasks()
		filtered_tasks = []
		for task in tasks:
			status = task['status']
			if status not in ['done']:
				filtered_tasks.append(task)
		return filtered_tasks

	def filter_completed_tasks():
		tasks = get_tasks()
		filtered_tasks = []
		for task in tasks:
			status = task['status']
			if status in ['done']:
				filtered_tasks.append(task)
		return filtered_tasks
```

In the above I looped through each object in the existing list, and each object whose status matches a given string is appended to an empty list.

### List comprehension:
Let's rewrite:

```python
	def filter_incomplete_tasks():
		return [task for task in get_tasks() if task['status'] != 'done']

	def filter_completed_tasks():
		return [task for task in get_tasks() if task['status'] == 'done']
```

Now the logic is more readable with only one line of code.

## Exceptions
Handling different types of errors in an application is a fundamental aspect so that in the event of an error, the application fails gracefully. In my case, I expect three areas where the app could fail:
- User enters an id that does not belong to any task in the list. A `TaskNotFoundError` is raised with error `Task not found with id {task_id}`.
- User inserts a non-numeric value for an id, which is invalid because the app expects a number, so an `InvalidTaskIdError` is raised with error `task id must be a number`.
- When the JSON data in the file is corrupted for some reason, e.g. an extra comma, missing square or curly brackets and so on, then a `TaskDataCorruptedError` is raised rather than silently failing.

All three custom exceptions above are defined in `python/exceptions.py` file as follows:

```python
	class TaskNotFoundError(Exception):
		pass

	class InvalidTaskIdError(Exception):
		pass

	class TaskDataCorruptedError(Exception):
		pass
```

## Handling User Input
All the functions and exceptions discussed earlier are implemented in the `python/` directory. The user input handling and command execution logic are implemented in the `tracker.py` file. It would be easier to divide the logic into categories and talk about each one:

### Command parsing
I used `sys` module to handle the actions the user wants to perform:

```python
	command = sys.argv[1]
```

`sys` is a built-in Python module that provides system-specific features, such as reading command-line arguments, interacting with input/output streams, etc.

`sys.argv[1]` is used to retrieve the first command-line argument passed to the script.

I used arguments with the app commands to trigger the corresponding action:
- `add` for adding tasks
- `list` for viewing tasks
- `update` for updating tasks
- `delete` for deleting tasks
- `progress` to mark tasks as `in-progress`
- `completed` to mark tasks as `done`
- `pending` to view incomplete tasks
- `done` to view completed tasks

I'll explain this more later through examples.

### User input validation
To ensure user input is valid before processing, I defined a function that takes the numeric input for the task id and raise an error if it fails to meet the condition:

```python
	def read_task_id():
		try:
			return int(input())
		except ValueError:
			raise InvalidTaskIdError("task id must be a number")
```
`InvalidTaskIdError` handles non-numeric input.

### Command execution
Using the `command`, the appropriate function can be triggered based on specific user command. For example:
- `python3 tracker.py add` triggers `add_task()` function
- `python3 tracker.py list`	triggers `get_tasks()` function
- `python3 tracker.py update` triggers `update_task()` function
and so on.


## To close
Although the project concept and components are not complex, I did learn (and relearn) a lot from it and it was a good opportunity to understand all the concepts through applying them in a real project.

The plan is to replicate the application in Java, which I hope would teach me a lot as well, even though it is already my standard go-to language for development.