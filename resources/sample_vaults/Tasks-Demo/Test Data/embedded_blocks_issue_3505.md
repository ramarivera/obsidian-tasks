# embedded_blocks_issue_3505

## Tasks
- [ ] #task Task 1 in 'embedded_blocks_issue_3505' WITHCODEBLOCK
    ```ts
    console.log("H")
    ```
    ^task-with-codeblock
- [ ] #task Task 2 in 'embedded_blocks_issue_3505' NOCODEBLOCK ^task-without-codeblock
- [ ] Task 3 in 'embedded_blocks_issue_3505' NOGLOBALFILTER
    ```ts
    console.log("H")
    ```
    ^task-with-codeblock-no-global-filter

## Embeds
![[embedded_blocks_issue_3505#^task-with-codeblock]]

![[embedded_blocks_issue_3505#^task-without-codeblock]]

![[embedded_blocks_issue_3505#^task-with-codeblock-no-global-filter]]


## Query view
```tasks
fileName includes 3505
group by function task.blockLink.replace(" ^","") 
```
