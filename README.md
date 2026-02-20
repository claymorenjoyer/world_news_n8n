# World News Automation

This n8n automation acts as a Telegram-based news aggregator. It allows users to request recent world news from specific major outlets via a Telegram bot, which then fetches, filters, and formats the headlines into a clean, mobile-friendly message.

## Core Functionality

The workflow follows a Request → Fetch → Process → Deliver logic:

### Request (Telegram Trigger):
The bot listens for specific commands: /bbc, /cnn, /reuters, /alj, or /guardian. If a command isn't recognized, it sends a helpful "Error message" listing the valid options.

### Routing (If Nodes):
A chain of If nodes checks which command was typed and routes the workflow to the corresponding RSS Feed Read node (e.g., BBC World, CNN World, etc.).

### Data Filtering (Code Nodes):

#### Time Filter: 
It removes any articles older than 7 days. If no articles are found in that window, it keeps the single newest one to ensure the bot doesn't return an empty respond.

#### Content Filter: 
It applies a "blocklist" to remove non-news items like puzzles (Quordle, NYT Strands) or advertisements (Black Friday, deals, sales).

#### Sorting: 
It sorts the remaining news by date (newest first) and limits the output to the top 10 items.

### Formatting (Code Node):
The raw RSS data is cleaned. It strips HTML tags, extracts the first five sentences as a summary, and formats the message with bold titles, numbered lists, and Markdown links.

### Delivery (Telegram Node):
The final formatted "World News" digest is sent back to a specific Telegram group.

## Key Technical Details

#### Dynamic RSS Fetching: 
Uses various official RSS feeds (e.g., https://feeds.bbci.co.uk/news/rss.xml).

#### Logic Resilience: 
Includes a "fallback" to the newest item if the 7-day filter returns nothing, preventing workflow execution errors.

#### Regex Cleaning: 
Uses JavaScript regex within the "format" node to handle common HTML entities like &amp; or &quot; for better readability.


## Demonstration:

![demonstration](https://github.com/user-attachments/assets/5ecd6114-2dda-4b5d-81c9-4c6ead02db14)
