# LinkedIn Automations (n8n)

Two [n8n](https://n8n.io) workflows for LinkedIn: one collects and rates job listings, the other helps produce LinkedIn posts. Both are exported as JSON files that you can import into n8n.

## `Linkedin_Job_Scraper.json` (workflow "Linkedin Job Scraper")

Started manually from n8n. It uses these nodes:

- **Start URL** sets the LinkedIn search to scrape.
- **Run Actor**, **Actor Completed?** and **Get Data** call the Apify API: they start an Apify actor, wait for it to finish and download the resulting dataset.
- **Get Airtable** and **Compare Datasets** compare the new results with jobs already stored in Airtable.
- **AI Job Summary** and **Match Level** are OpenAI nodes that summarise a job and rate how well it matches.
- **Airtable** saves the results.

## `Linkedin_Post_Generator.json` (workflow "Automated LinkedIN")

Runs on a schedule. It uses these nodes:

- **Get articles** reads an RSS feed. **Filter articles about your niche**, **Drop articles that are not about our niche**, **Filter out already scraped articels** and **Pick 1 Article** narrow it to one new relevant article, using OpenAI and Google Sheets to remember what has been used.
- **Perplexity** gets more information about the article, and an OpenAI node splits the response into data.
- **Creating LinkedIn ideas** and **Create LinkedIn post** are agents that use Anthropic chat models with a structured output parser.
- **Google Sheets** nodes store the ideas and approvals.
- **Approve the Post** sends an email through Gmail.

## Use

1. In n8n choose **Import from file** and select a workflow JSON.
2. Add your own credentials for the services each workflow uses: Apify, Airtable and OpenAI for the job scraper; OpenAI, Anthropic, Perplexity, Google Sheets and Gmail for the post generator.
3. Update the Start URL, spreadsheet, Airtable and RSS settings to match your own accounts.
