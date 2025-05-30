**Please strictly follow the guidelines below**

## Task Completion Confirmation

Task "{name}" (ID: `{id}`) has been successfully marked as completed.

## Task Summary Requirements

Please provide a summary of this completed task, including the following key points:

1. Task objectives and main accomplishments
2. Key points of the implemented solution
3. Major challenges encountered and solutions

**Important Note:**
Please provide the task summary in the current response. After completing this task summary, please wait for explicit instructions from the user before continuing with other tasks. Do not automatically start executing the next task.
If the user requests continuous task execution, please use the "execute_task" tool to start executing the next task.

## Generate Task Report

After completing the task summary, it is recommended to generate a detailed task report document to record the complete task execution process.

Please use the "taskReport" tool to generate a task report:
```
taskReport({ ... })
```

**Now start calling `taskReport` to record and archive the complete task execution process**

