---
title: Funder data source review
tags: FunderData
toc: true
show_landing: false
created: 2021-08-11 15:56
---

# Funder data source review

## Meta

This document reviews sites, rates them with respect to expected processing difficulty, and notes other interesting details.

### Processing notes

* best to pull the raw data first, then process it? (Means we only have to pull data once.)
* if so, can we still leverage Scrapy (either for pulling, or processing)?
* common fields, for canonical data model: likely limited to:
    * grant amount (TODO: note currency; can then translate amounts as needed)
    * grantee
    * funding area
    * funding year
    * funder
* (still, can also save full information in per-funder tables/indexes and save the harmonized/canonical fields for all-org searches)

### Storage notes

* stash in Postgres? or, ElasticSearch?

---

## Key funders

ORFG == Open Research Funding Group (<http://www.orfg.org/members>)

### Alfred P. Sloan Foundation (member ORFG)

* website: <https://sloan.org/grants-database>
* already handled by our existing tools? Yes
* has API? No
* difficulty: Medium
* notes:
    * difficulty is mostly due to size, though can whittle that down if we find a way to (programmatically) activate the "year" and "program" checkboxes on the side. Maybe even manually set these, choose 50 records per page, and download the raw HTML (since they only do about 50 grants per year and we probably won't need to go back _too_ far ...)
* todo: check our existing tools to see how it handles the points mentioned above re: difficulty
* data notes:
    * Pulled raw data? **Yes** (manual, through 2017)
    * Parsed/extracted data? Yes

### American Heart Association (member ORFG)

* website: <http://www.heart.org/HEARTORG/>
* already handled by our existing tools? No
* has API?
* difficulty:
* notes:
* todo: need to find this one. The URL has changed, the WayBack Machine is down, and nothing on their site stands out
* data notes:
    * Pulled raw data? **No**
    * Parsed/extracted data? **No**

### Arcadia (member ORFG)

* website: <https://www.arcadiafund.org.uk/grant-directory>
* already handled by our existing tools?
* has API? No
* difficulty: low
* notes:
    * data is available as an Excel sheet! [https://www.arcadiafund.org.uk/uploads/Arcadia-grants-360Giving-20-September-2020.xlsx](https://www.arcadiafund.org.uk/uploads/Arcadia-grants-360Giving-20-September-2020.xlsx)
    * (Note that the filename isn't predictable; for future revisions of this app, we can choose between _"manually download this once a quarter or so"_ and _"build an app to check the link."_  The latter, while it may appear smoother, has enough of its own issues that it's likely not worth the trouble.)
    * "The information is licensed under the Creative Commons Attribution 4.0 International License. This means the data is freely accessible to anyone to use and share, as long as it is attributed to Arcadia Fund."
    * Based on the website/search tool, looks like the grant amounts are in USD (not GBP)
* data notes:
    * Pulled raw data? **Yes** (It's in a spreadsheet)
    * Parsed/extracted data? **Yes**

### Arnold Ventures (member ORFG)

* website: <https://www.arnoldventures.org/grants-list/>
* already handled by our existing tools?
* has API? No
* difficulty: medium
* notes:
    * plus side: predictable URLs, e.g. second page of grants list is at <https://www.arnoldventures.org/grants-list/p2>
    * grant info sections are not terribly detailed (search results don't have links to detailed info; WYSIWYG on the search page)
* data notes:
    * Pulled raw data? **Yes**
    * Parsed/extracted data? **Yes**

### Bill & Melinda Gates Foundation (member ORFG)

* website: <https://www.gatesfoundation.org/How-We-Work/Quick-Links/Grants-Database>
* has API? No
* difficulty: Medium
* notes:
    * will require creative scraping: first the top-level search pages, to get the list of grant links; then each grant link, to get the details of what the grant was for. (If we only care about the grantee, amount, issue, and program, then we can just grab the search pages)
    * can limit our collection if we narrow our search to a given issue, e.g., <https://www.gatesfoundation.org/How-We-Work/Quick-Links/Grants-Database#q/issue=Global%20Libraries>
* data notes:
    * Pulled raw data? **No**   (site down for maintenance 2020/10/26) (site _still_ down for maintenance 2020/10/31) (update 2020/12: raw data not accessible)
    * Parsed/extracted data? **No**

### Eric & Wendy Schmidt Fund for Strategic Innovation (member ORFG)

* website: <https://tsffoundation.org/>
* already handled by our existing tools? No
* has API? No
* difficulty: High
* notes:
    * they seem very quiet about what they fund. Might need to extract data from 990s
* data notes:
    * Pulled raw data? **No**
    * Parsed/extracted data? **No**

### Gordon and Betty Moore Foundation (member ORFG)

* website: <https://www.moore.org/grants>
* already handled by our existing tools? No
* has API? No
* difficulty: Medium
* notes:
    * spidering will need to dig into detail pages.  (The main search results page shows everything we need … _except for_ the grant's category/area.)
    * on each search results page, look for:
        * `detail pages: /grant-detail?grantId=`
    * to limit our pull, we can search by year and ask to see all results for that year:: <https://www.moore.org/grants>?**showAll=true**&**filterYear=2020**&searchFunction=StartsWith&searchFields=Title#filterSortBarPageJumper
    *
* data notes:
    * Pulled raw data? **Yes**
    * Parsed/extracted data? **Yes**

### Howard Hughes Medical Institute (HHMI) (member ORFG)

* website: <https://www.hhmi.org/>
* already handled by our existing tools? No
* has API? No
* difficulty: ?
* notes:
    * no obvious list of grants, nor anything of detail in the published financials on the website; may need to scrape the raw 990s
* data notes:
    * Pulled raw data? **No**
    * Parsed/extracted data? **No**

### James S. McDonnell Foundation (member ORFG)

* website: <https://www.jsmf.org/grants/index.php>
* already handled by our existing tools? No
* has API? No
* difficulty: Medium
* notes:
    * predictable URLs to search by year: <https://www.jsmf.org/grants/index.php?year=2018>
    * a quick skim didn't yield any pagination, so in theory, scraping the top-level pages (and using those to pull the links) should be straightforward
* data notes:
    * Pulled raw data? **Yes**
    * Parsed/extracted data? **Yes**

### John Templeton Foundation (member ORFG)

* website: <https://www.templeton.org/grants/grant-database>
* already handled by our existing tools? No
* has API? No
* difficulty: Low
* notes:
    * while each grant has a detail page, it's more of a short write-up on the project. (aka, the funding amount and funding area are on the main search page.) Possibly useful to us down the road, but not now.
    * based on a quick skim, their funding areas -- "Science & the big questions," "Character Virtue Development," "Individual Freedom & free markets," "Exceptional cognitive talent & genius," "Genetics," "Voluntary family planning" -- may not overlap too much with our infrastructure focus
    * the entire grants database is in the single webpage; the "pagination" is really JavaScript that scrolls through the data that's already embedded in the single-page HTML.  Hence, there's no code needed to "pull" this data; we can manually download the HTML and be done with it.
* data notes:
    * Pulled raw data? **Yes**
    * Parsed/extracted data? **Yes**

### The Leona M. and Harry B. Helmsley Charitable Trust (member ORFG)

* website: <https://helmsleytrust.org/our-grants>
* already handled by our existing tools? No
* has API?
* difficulty: Medium or High
* notes:
    * detail pages include the term/duration, and have a brief blurb on the project ... but aside from that, all of the meat is on the main search results page
    * the search pages seem to use JavaScript to render the search results.  That means the search result pages don't have the grant info in the raw HTML…
* data notes:
    * Pulled raw data? **No**
    * Parsed/extracted data? **No**

### Lumina Foundation (member ORFG)

* website: <https://www.luminafoundation.org/resources/grants/grant-database/>
* already handled by our existing tools? No
* has API? No
* difficulty: Medium
* notes:
    * search page results format is odd (tiles, not rows) but might be fine from an HTML-parsing standpoint
    * grant detail pages have no additional info beyond what's on the search results pages
    * only ten grants per page; may take a lot of requests to collect
    * search results page has predictable URL format: <https://www.luminafoundation.org/resources/grants/grant-database/page/3/>
    * many of the grant period/duration aren't really ranges, just individual dates
* data notes:
    * Pulled raw data? **Yes**   _(first 70 pages of results; can go back for more if needed)_
    * Parsed/extracted data? **Yes**

### Open Society Foundations (member ORFG)

* website: <https://www.opensocietyfoundations.org/grants/past>
* already handled by our existing tools? No
* has API? No
* difficulty: Medium
* notes:
    * grants search page only covers 2016, 2017, 2018
    * more recent, yet-to-be awarded grants are listed on <https://www.opensocietyfoundations.org/grants> but there's no info there w/r/t grant amount and the like
    * all grant details are in the search results (there are no detail pages)
    * relevant to our interests: they list total 565 grants under "Higher Education" and "Information and Digital Rights"
    * for the main search page (no filters → getting _all_ grants):
        * there's no true "pagination"; instead, each click of "show more grants" uses JavaScript to append more data to the current search results
        * URLs are predictable; so if we wanted, say, 200 pages' of results we would use: <https://www.opensocietyfoundations.org/grants/past?page=200>
        * combining those last two: this means crawling is less of an option; probably best to manually specify some (high) number of "pages" and save the raw HTML for later parsing
    * if we provide search criteria (e.g., by program, such as HESP)
        * "hesp" == "higher education support program"
        * In this case, the "show more grants" link does proper pagination
        * e.g.., <https://www.opensocietyfoundations.org/grants/past?filter_program=hesp%2Cinformation-program&page=15>
* data notes:
    * Pulled raw data? **No**
    * Parsed/extracted data? **No**

### Rita Allen Foundation (member ORFG)

* website: <https://ritaallen.org/all-grants/>
* already handled by our existing tools? No
* has API? No
* difficulty: Medium
* notes:
    * grant search page has predictable URLs, and they are broken down by year: <https://ritaallen.org/grant-year/2019/>
    * due to page formatting, will require additional work to capture a given grant's area (as they are grouped under common headings, instead of a tabular view that lists the area alongside the other grant details)
    * grants do not have detail pages, so we'd only have to pull the search result pages
    * only lists grants for 2010-2019; 2020 is not (yet) present, not even if we hit the 2020 URL directly
* data notes:
    * Pulled raw data?  **Yes**
    * Parsed/extracted data? **Yes**

### Robert Wood Johnson Foundation (member ORFG)

* website: <https://www.rwjf.org/en/how-we-work/grants-explorer.html>
* already handled by our existing tools? No
* has API? No
* difficulty: Medium
* notes:
    * the search results page has all of the information (click to expand a section); there are no detail pages
    * search result pagination has predictable URLs, e.g. page 5648 is at: <https://www.rwjf.org/en/how-we-work/grants-explorer.html#s=5648>
    * grants database goes back to 1972 (and, thousands of grants awarded); would likely want to limit to recent years
    * their interest areas focus on health, which doesn't have a ton of overlap with our interest areas
    * we can export data as a CSV; no spider needed
    * even better: at first glance, the CSV looks fairly detailed
* data notes:
    * Pulled raw data? **Yes**
    * Parsed/extracted data? **Yes**

### Templeton World Charity Foundation (member ORFG)

* website: <https://www.templetonworldcharity.org/projects-database>
* already handled by our existing tools? No
* has API?
* difficulty:
* notes:
    * will need to pull data from detail pages; so, two-pass collection: search pages (to get detail URLs) then detail pages
    * areas of initiatives, of interest to us (per spreadsheet): "Accelerating Research on Consciousness," "Big Questions in Classrooms"
    * the search result pages seem to use JavaScript to render the results, which means they don't appear in the raw HTML source.
        * then again, there are only a handful of grants for our interest areas … so with a few clicks we can manually copy the links to the detail pages and then spider those accordingly
* data notes:
    * Pulled raw data? **Yes**
    * Parsed/extracted data? **Yes**

### Wellcome (member ORFG)

* website: <https://wellcome.org/grant-funding/people-and-projects/grants-awarded>
* already handled by our existing tools? No
* has API? No
* difficulty: ?
* notes:
    * neither the search results page, nor the detail page, shows the amount awarded
    * even if there were useful information on the search result pages, there are ~2000 records (120 pages) of results related to our topic of interest (Biomedical Research) which would be a large order for crawling.
* data notes:
    * Pulled raw data? **No_ -- perhaps, skip? (see above)_**
    * Parsed/extracted data? **No**

### Mellon

* website: <https://mellon.org/grants/grants-database/>
* already handled by our existing tools? No
* has API? No
* difficulty: Medium
* notes:
    * data goes back to 1980; would likely need to limit our search criteria to recent years
    * two-pass collection: search results (to pull links to detail pages) and then detail pages
        * the search results page only lets us limit to "2010-present" time frame, which likely includes a lot more data than we'd want
        * as such, we'd want to pull the top-level search result pages, then build tools to extract the target URLs for our years of choice
        * only additional information on detail pages:
            * Area of Focus (not the same as Program Area, which is on the search results page)
            * Duration (in months)
            * brief (one- or two-sentence) blurb on the grant
            * reference number (which we can also extract from the detail page URL)
        * **sum total: maybe skip the detail pages?** a lot of extra crawling and extraction, for not a lot more data
    * they even have a "higher learning" category
    * for search results: can specify items per page as URL param per_page= (up to 100)
    *
    * can specify program/area using URL parameter p= (can specify multiple times for multiple programs)
        * program numbers: 109 = Higher Education , 114 = Public Knowledge
    * predictable URLs, e.g.:
        * <code>[https://mellon.org/grants/grants-database/?p=106&p=109&grantee=&q=&s=&n=&e=&w=&z=2&lat=22.7231920&lon=-73.9529910&per_page=100](https://mellon.org/grants/grants-database/?p=106&p=109&grantee=&q=&s=&n=&e=&w=&z=2&lat=22.7231920&lon=-73.9529910&per_page=100)</code>
        * <code>our start URL: [https://mellon.org/grants/grants-database/?p=109&p=114&grantee=&y=2010-2020&q=&s=-42.44844747910975&n=67.92770824406576&e=180&w=-180&z=2&lat=22.7231920&lon=-73.9529910&per_page=100](https://mellon.org/grants/grants-database/?p=109&p=114&grantee=&y=2010-2020&q=&s=-42.44844747910975&n=67.92770824406576&e=180&w=-180&z=2&lat=22.7231920&lon=-73.9529910&per_page=100)</code>
        * <code>our end URL (note page= param) [https://mellon.org/grants/grants-database/?page=30&e=180&grantee=&lon=-73.9529910&n=67.92770824406576&q=&p=109&p=114&s=-42.44844747910975&w=-180&y=2010-2020&per_page=100&z=2&lat=22.7231920](https://mellon.org/grants/grants-database/?page=30&e=180&grantee=&lon=-73.9529910&n=67.92770824406576&q=&p=109&p=114&s=-42.44844747910975&w=-180&y=2010-2020&per_page=100&z=2&lat=22.7231920)</code>
        *
* data notes:
    * Pulled raw data? <strong>Yes</strong>  <em>(just the search result pages; see note above re: skipping detail pages)</em>
    * Parsed/extracted data? <strong>Yes</strong>

### Siegel Family Endowment

* website: <https://www.siegelendowment.org/grantees/>
* already handled by our existing tools? No
* has API? No
* difficulty: ?
* notes:
    * no substantive grants info that I can find on the site
    * TODO: double-check Dave's code; could've sworn this was covered (which would imply that there's grants info in there somewhere)
* data notes:
    * Pulled raw data?  **No**
    * Parsed/extracted data? **No**

### Chan Zuckerberg Initiative

* website: <https://chanzuckerberg.com/grants-ventures/grants/>
* already handled by our existing tools? Yes
* has API? No
* difficulty: Medium
* notes:
    * all info is available on an (infinite-scroll) page ... so, possible to hit this in a browser, scroll to the very end, and save that file for later processing
    * there are no detail pages; just the search results page
* data notes:
    * Pulled raw data? **Yes**
    * Parsed/extracted data? **Yes**

### FundRef

* website: <https://www.crossref.org/services/funder-registry/>
* already handled by our existing tools? No
* has API? Yes
* difficulty: Low
* notes:
    * looks like a useful resource for us to check for other data

### IMLS

* website: <https://www.imls.gov/grants/awarded-grants>
* already handled by our existing tools? No
* has API? No
* difficulty: Low
* notes:
    * we can manually do a search with no criteria, then click the "download result as CSV" button
    * while the search results on the website are paginated, the CSV has _all_ results
    * I ran this 2020/10/14 and have the data
* data notes:
    * Pulled raw data? **Yes**
    * Parsed/extracted data? **Yes**

### NEH

* website: <https://securegrants.neh.gov/publicquery/main.aspx>
* already handled by our existing tools? No
* has API? Yes (but … see below)
* difficulty: Low
* notes:
    * API instructions are available at [https://securegrants.neh.gov/publicquery/api.pdf](https://securegrants.neh.gov/publicquery/api.pdf)
    * **per "Has API?" above:**while there's an API, the (manual) web form also lets us save search results as an Excel file.  Easier for us to extract from an Excel sheet than to parse the raw HTML that comes back from an API search call.
    * grant details show approved vs awarded amounts... will need to factor this in w/r/t data model
    * while detail pages _technically_ exist, they're really "single search result" pages.  aka they have the exact same per-grant info as in the wider search result pages… so there's no need to pull the detail pages.
    *
* data notes:
    * Pulled raw data? **Yes -- but need to get the rest** (pulled a sample, to test parsing)
    * Parsed/extracted data? **Yes**

### NSF

* website: <https://www.nsf.gov/awardsearch/advancedSearch.jsp>
* already handled by our existing tools? No
* has API? No
* difficulty: Low
* notes:
    * we can manually enter search(es) and download the results as CSV or XML
    * or, if we'd prefer to see all awards, we can download XML datafiles at [https://www.nsf.gov/awardsearch/download.jsp](https://www.nsf.gov/awardsearch/download.jsp)
        * we can grab files year-by-year (notice the `DownloadFileName` parameter): [https://www.nsf.gov/awardsearch/download?DownloadFileName=2020&All=true](https://www.nsf.gov/awardsearch/download?DownloadFileName=2020&All=true)
        * (note that downloads are pretty slow .. 1-2mn for a 20MB file.  Plan accordingly.)
        * XML schema is at: <https://www.nsf.gov/awardsearch/resources/Award.xsd>
* data notes:
    * Pulled raw data? **Yes -- need to go back for the rest** (grabbed 2020 docs, in XML, to test parsing)
    * Parsed/extracted data? **Yes**

### NIH

* website: <https://orip.nih.gov/funding/search-awarded-grants>
* already handled by our existing tools? No
* has API? No
* difficulty: Low
* notes:
    * we can download raw datafiles from <https://exporter.nih.gov/>
    * TODO re: above: will need to fish out the grants, as this looks like all NIH projects in one spot
* data notes:
    * Pulled raw data? **Yes -- need to go back for the rest** (pulled 2019 data, in CSV, to test parsing)
    * Parsed/extracted data? **Yes**

### DOE

* website: <https://www.energy.gov/science/office-science-funding/office-science-awards>
* already handled by our existing tools? No
* has API? No
* difficulty: Low
* notes:
    * we can download raw data (Excel format) from [https://science.osti.gov/Universities/sc-in-your-state/](https://science.osti.gov/Universities/sc-in-your-state/)
* data notes:
    * Pulled raw data? **Yes  -- need to go back for the rest** (pulled a sample from FY2019, in Excel, to test parsing)
    * Parsed/extracted data? **Yes**

### European Union

* website: <https://ec.europa.eu/budget/fts/index_en.htm>
* already handled by our existing tools? No
* has API?
* difficulty:
* notes:
    * To see all grants, click "search" without providing any criteria
    * The results page has an export option.  If you click that, a form will ask for your e-mail address to retrieve your results by e-mail.   \
 \
At the bottom of that page, though, is a link to download the data in raw form.  We can use that to manually pull files.
    * (Still, we'd need to download one file per year of interest.  I don't see an immediate way to use automated tools; and depending on how many years we want, it may not be worth the effort to do so.)
    * The data files are arranged in such a way that a single grant ("commitment") may have multiple beneficiaries, and they don't break down amounts by beneficiary.
    * That said, a quick check reveals no grants issued to US-based organizations for 2017, 2018, 2019 … so … maybe not for us anyway?
* data notes:
    * Pulled raw data? **Yes** (a sample from 2019, in XML, to test parsing)
    * Parsed/extracted data?  **No**(see above)

### TODO: JISC

* website: <https://www.jisc.ac.uk/rd/projects>
* already handled by our existing tools?
* has API?
* difficulty:
* notes:
    * can show all projects (all years) in a single page using: [https://www.jisc.ac.uk/rd/projects/archived?sort_by=viewed&items_per_page=All&search_api_views_fulltext_projects=](https://www.jisc.ac.uk/rd/projects/archived?sort_by=viewed&items_per_page=All&search_api_views_fulltext_projects=)
    * from there, we can use automated tools to pull the detail pages
    * That said, neither the search results page nor the detail pages include award amounts.  (The detail pages are really short articles on the project.  Lots of detail, but not really machine-friendly.)
* data notes:
    * Pulled raw data?
    * Parsed/extracted data?

### TODO: Australian Research Council

* website: [https://www.arc.gov.au/grants](https://www.arc.gov.au/grants)
* notes:
    * Can download an Excel file with the data: [https://www.arc.gov.au/grants-and-funding/apply-funding/grants-dataset](https://www.arc.gov.au/grants-and-funding/apply-funding/grants-dataset)  and scroll down to "NCGP Projects"
* data notes:
    * Pulled raw data? **Yes**
    * Parsed/extracted data? **Yes**

### TODO: NHMRC (Australia)

* website: <https://www.nhmrc.gov.au/funding>
* already handled by our existing tools? No
* has API? No
* difficulty: Low - data is in Excel spreadsheets
* notes:
    * can grab spreadsheets here: [https://www.nhmrc.gov.au/funding/data-research/outcomes-funding-rounds](https://www.nhmrc.gov.au/funding/data-research/outcomes-funding-rounds)
    * spreadsheets are provided by funding year.  Given the URL scheme, and that there are only 10 or so files, it's best we download these manually (for later post-processing)
* data notes:
    * Pulled raw data?
    * Parsed/extracted data?

### TODO: UK Gateway to Research

* website:
* already handled by our existing tools?
* has API?
* difficulty:
* notes:
    * gets high marks from Cameron
* data notes:
    * Pulled raw data?
    * Parsed/extracted data?

### TODO: EuropePMC

* website:
* already handled by our existing tools?
* has API?
* difficulty:
* notes:
* data notes:
    * Pulled raw data?
    * Parsed/extracted data?
