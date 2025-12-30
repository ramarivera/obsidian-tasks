# Pairing notes

## 2025-11-25

- Appending child elements in the `else` branch fixes the lost of the codeblock in reading mode and live preview embeds (which reuses reading mode logic)
    - But it creates a duplicate of the line in reading mode 
    - and the global filter is present 
        - or its appended at the end of the line before rendering the code block -> *BREAKS remove global filter from description setting*
    - and in reading and live preview embeds, there is no reading mode line duplicate
    

### What to do next
- Add some css to make task plugin specific classes visible
    - The `else` branch caused the plugin to re render the whole line, instead of just the children, therefore we need to understand that part of the code better
### With no changes

```diff
diff --git a/src/Obsidian/InlineRenderer.ts b/src/Obsidian/InlineRenderer.ts
index 4fdbc048..199809e4 100644
--- a/src/Obsidian/InlineRenderer.ts
+++ b/src/Obsidian/InlineRenderer.ts
@@ -101,7 +101,7 @@ export class InlineRenderer {
 
             const precedingHeader = null; // We don't need the preceding header for in-line rendering.
             const task = Task.fromLine({
-                line,
+                line: line + 'XYZ',
                 taskLocation: new TaskLocation(
                     new TasksFile(path),
                     lineNumber,

```


![[CleanShot 2025-12-30 at 09.33.17@2x.png]]

With no changes BUT the red border marker

![[CleanShot 2025-12-30 at 10.08.50@2x.png]]

### Code changes

With the following diff

```diff
diff --git a/src/Obsidian/InlineRenderer.ts b/src/Obsidian/InlineRenderer.ts
index 4fdbc048..85c955f3 100644
--- a/src/Obsidian/InlineRenderer.ts
+++ b/src/Obsidian/InlineRenderer.ts
@@ -101,7 +101,7 @@ export class InlineRenderer {
 
             const precedingHeader = null; // We don't need the preceding header for in-line rendering.
             const task = Task.fromLine({
-                line,
+                line: line + 'XYZ',
                 taskLocation: new TaskLocation(
                     new TasksFile(path),
                     lineNumber,
@@ -154,6 +154,8 @@ export class InlineRenderer {
                     taskElement.prepend(renderedChild);
                 } else if (nodeName === 'ul' || nodeName === 'ol') {
                     taskElement.append(renderedChild);
+                } else {
+                    taskElement.append(renderedChild);
                 }
             }
 

```

we get

![[CleanShot 2025-12-30 at 09.27.03@2x.png]]

And this is with the css-editor plugin marking the rendered tasks with a red border

![[CleanShot 2025-12-30 at 10.05.22@2x.png]]

Observe how it affects the embeds on live preview
## 2025-12-30

- On query view, for the task line with the codeblock AND the blockid after the codeblock, the query view is NOT rendering/grouping by blockid 
    - Reason: Tasks code determines the task's blockid by looking at the end of the task line -> too primitive
        - Obsidian's cache metadata has the blockid so we could leverage it
- We saw the original task lines duplicates on reading view when we started this session, but after making some edits, by the time we took the previous screenshots, the duplicates were gone -> WHY???????
- When trying to click on the checkbox of the first task line in the embed view on reading mode.... nothing happened, and this message popped up
    - ![[CleanShot 2025-12-30 at 09.49.01@2x.png]]
- When clicking on the checkbox of the third NOT task line on reading mode embed, it marked all occurrences as "done", but when clicking again to undo it, it undid all lines EXCEPT for the live preview embed
    - ![[CleanShot 2025-12-30 at 09.52.10@2x.png]]
    - **The same thing happens on restricted mode*** -> therefore it is not something we will be able to 100% fix