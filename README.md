# charter_html_analysis
Analyzing the html structure of the SF Charter

### Doc Structure Notes
Example section
```html
<div>
    <div id="rid-0-0-0-20" class="Section">
        <div>
            <a name="JD_1.100" id="JD_1.100" title="1.100"></a>
            "SEC. 1.100. NAME AND BOUNDARIES"
        </div>
    </div>
    <div id="rid-0-0-0-21" class="Normal-level">
        <div>
            "The City and County of San Francisco shall continue as a consolidated City and County..."
        </div>
    </div>
</div>
```
- The `<body>` is all `div`s on the same level, no nesting for Articles vs Sections
- Every paragraph is a `div` with a unique `id` but they're sequential, so no logical separation
- Seems like every "header" (article, section, etc) has an empty `<a>` element with a `title` attribute
    - `title` attribute seems like the most reliable handle

### Misc Notes
- `title=""` seems like a mistake, appears twice:
    - SEC. 8A.101.  MUNICIPAL TRANSPORTATION AGENCY. Subsection (b)
    - A8.423  REVISION OF SCHEDULES AND COMPENSATION