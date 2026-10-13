Architecture Specification - ARA-1

Primary Agent Pattern:

I choose Plan-and-Execute as my primary agent pattern with ReAct fallback. because it handless complex research tasks better.
Our Financial research will have so many steps like calculating SEC fillings ratios and news checking etc. so I think if we choose Plan-and-Execute our research will work systematically.it executes step by step itself so it is easy to monitor and update for us.by using it we can avoid gaps while creating reports.
When we do financial research, doing everything in one go is very hard. So choosing Plan-and-Execute helps us break down the big job into small parts first. It works like making a complete daily list before starting work. If any tool fails or gives an error while working, the ReAct fallback comes to help. It thinks quickly and tries another way to get the data without stopping the whole process. This way we can calculate all numbers correctly and write full reports without missing any small detail.

Cognitive Loop Design:

Query receiving :

we use LLM with query decomposition prompt and make 3-5 specific sub queries . the sub queries research financial data of target companies and parse financial metrics
When a big question comes from the user, the agent does not start searching randomly. It uses a prompt to break the main question into 3 to 5 small easy sub queries. For example, one sub query looks at company balance sheets, another checks profit numbers, and another checks company news. Breaking a big task into small sub queries helps us get exact answers for each part without confusion.

Planning :

we use planner LLN it creates complete sequential research plan before invoking any tools it is very helpful to do our research work
The planner LLM takes all the small sub queries and arranges them in a step-by-step order before using any tools. It decides what to do first and what to do next. For example, it gets past financial numbers first before comparing them with competitors. Making a clear plan first saves time, prevents repeating the same work, and makes it easy for us to check if the plan is right.

Tool Execution :

we use Executor LLM, it selects specific tool from our tool registry which is needed for a specific plan.
After the plan is ready, the executor LLM takes action step by step. It looks at the current step in the plan and chooses the exact tool needed from our tool registry. If it needs SEC file reader, it picks that tool. If it needs a news finder, it picks the news tool. It runs the tool, gets the required result, and sends that information to the next step.

Observation and synthesis :

we use Multi-Stage RAG Pipeline. it evaluates the outputs, matches data from multiple sources and checks for data gaps and compiles final narrative.
Once all tools give their outputs, our pipeline checks all gathered information carefully. It compares numbers from different sources to make sure they match and there are no mistakes. If any detail is missing, it tries to fill that gap. Finally, it joins all these small pieces of information together into one simple, clear financial report story that anyone can read easily.

Self verification :

we use Chain-of-Verification (CoVe) prompt pattern. it helps the agent to re examine and verify the final report
To make sure there are no wrong statements or false numbers in our final report, we use the Chain-of-Verification prompt pattern. Before sending the report to the user, the agent asks itself questions about its own work. It re checks all numbers and facts against the original data. If it finds any wrong number or mistake, it corrects the mistake immediately and updates the final report.

Memory Architecture:

Short-Term-Memory :

user context window. it manages active chat session .Short-term memory works inside the active chat window. It remembers what the user asked, what the agent is currently doing, and what steps were just finished in the live chat session. When the chat gets too long with too much data, it summarizes older messages so that the chat window never gets full and the agent keeps working smoothly without forgetting current details.

Long-Term-Memory :

we use Chromadb for long term memory. it stores historical SEC fillings and reports. it finds and retrieves the past financial research sessions very fast and connects them with other sessions.
We use Chroma DB as our long-term memory to save old financial reports and SEC documents permanently. When we ask a question about old company history or past year trends, Chroma DB helps the agent find that past information very quickly. It connects old research work with new research work so we do not have to search for the same old files again and again from scratch.

Episodic Memory :

we use local storage. it stores past secaniors like which tools gave better results and stores them. it helps the agent to choose future plans smartly.
We use local storage to keep track of past experience. It saves records of which tools worked best and which plans gave fast results for past tasks. When a new task comes, the agent looks at this past memory to see what worked well before. This helps the agent make smarter decisions, pick the best tools quickly, and avoid old mistakes.

Memory Workflow:

when new query came the agent checks Episodic memory and and selects correct strategies that worked for specific past queries.
When a new request arrives, the agent first looks into its episodic memory. It checks if a similar question was solved in the past and sees which tools and steps worked best back then. By using those proven past strategies, the agent starts working smartly right away without trying wrong methods.

it sends the query to Chromadb (long term memory)
Next, the agent sends the query to Chroma DB to fetch old financial reports and company history. This brings all past facts and baseline numbers into picture, ensuring the agent bases its report on solid past background data rather than just guessing.

it loads the output data to context window (short term memory).
Finally, all gathered details from episodic memory and long-term memory are loaded into the short-term chat context window. The agent uses this working area to run tools, calculate numbers, check facts, and write the final report step by step for the user.

Complete 10-Tool Registry & Why We Choose Them (ARA-1):

sec_filing_search : Retrieves official SEC filings (10-K, 10-Q, 8-K) from EDGAR to get legal and true financial numbers. This helps the agent fetch genuine corporate numbers directly from source.

web_search : Performs web search for current news, market commentary, and public updates about target companies. This helps the agent stay updated with latest live market events.

earnings_transcript : Pulls company earnings call transcripts to understand executive plans and future guidance directly. This helps the agent capture management statements and corporate outlooks.

company_profile : Fetches company background details, industry sector, and market capital info using financial data APIs. This helps the agent understand basic corporate background and scale.

peer_comparison : Identifies industry competitors and compares key financial ratios side by side. This helps the agent evaluate relative market performance against competitors.

financial_ratios : Calculates core financial ratios like debt-to-equity, profit margins, and return on investment automatically. This helps the agent measure profitability and financial health without manual errors.

stock_price_history : Pulls historical stock price data and market trends over various timeframes. This helps the agent track long-term valuation trends and market performance.

sentiment_analyzer : Evaluates market news and social posts to check public sentiment on company stocks. This helps the agent gauge investor mood and public perception.

risk_assessment : Analyzes filing risk factor sections (Item 1A) to identify potential business threats. This helps the agent highlight key vulnerabilities and operational risks.

report_compiler : Formats and structures all gathered analytical outputs into a clean final report document. This helps the agent present clear, professional, and well-structured financial insights to the user.