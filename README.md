# charter_html_analysis
Analyzing the html structure of the SF Charter

Update: did a lot of hacking and slashing, not worth the time. Moving to simple text file for now

### Doc Structure Notes
General structure:
```html
<div>
    <div id="rid-0-0-0-18" class="Chapter">
        <div>
            <a name="JD_ArticleI" id="JD_ArticleI" title="Article I"></a>
            "ARTICLE I:"
            <br>
            <br>
            "EXISTENCE AND POWERS OF THE CITY AND COUTNY"
        </div>
    </div>
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
- Many sections have a `div` on the end at the same level which contains a `<div class="History">(Amended November 2001)</div>`
    - Keep these or ditch them? TODO: depends on text structure
    - There can be multiple History divs in a row, can't count on just one

### Misc Notes
- `class="* Chapter"` reliably in all divs for main sections (Preamble, Articles, Appendicies) but also includes Charter title page, SF Codes, Preface, and Appendicies
    - to catch Preamble text but discard Article tables, ~~I can filter on `class="xsl-table"`... xsl?~~ No I can't, Article 16 has tables
- inside a `class="Section"` div is sometimes a `<div class="Section-Deleted">` need to skip
- `title=""` seems like a mistake, appears twice:
    - SEC. 8A.101.  MUNICIPAL TRANSPORTATION AGENCY. Subsection (b)
    - A8.423  REVISION OF SCHEDULES AND COMPENSATION
- `title="16.127-5 Note 1"` is a footnote "So in Proposition C, approved November 14"
    - lives inside a `<div class="Codification-Note">` so I could filter that out first
    - I chose to filter out any title with "Note" substring instead, captured in separate array
- Need to handle links to other parts of the code. Either maintain or just replace with text
    - `In addition to its duties under <intercodelink destinationid="JD_Ch.5Art.V" doc="{{ origDocIdx }}" infobasepath="Administrative.nfo" linkprefix="/codes/san_francisco/latest/" stylename="Jump">Article V of Chapter 5</intercodelink> of the Administrative Code`
- `Section-Deleted` for 15.104 lives inside the same div as active section 15.103... ffs
    - I could just delete the specific div but that would leave stray history notes and Editor's notes behind

TODO
- Trash `<div class="EdNote">` Editor's Notes, often for repealed sections (Ex: 4.116)