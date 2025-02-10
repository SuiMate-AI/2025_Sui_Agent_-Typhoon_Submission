# SuiMate Universe AI Agents:<br>AIdeniyi + SuiTrivia + SuiTipper

SuiMate's vision is to become a trusted companion for every Web3 beginner, guiding them through the world of Web3 while educating them on Web3, DeFi, and blockchain concepts. To achieve this, we are developing a series of AI agents designed to engage and interact with users. The current three agents are just the beginning.


We originally planned to integrate all features to [AIdeniyi](https://x.com/aideniyi) but found that this would cause the agent to hit the rate limit easily. Therefore, we created three agents separately as follows:

- **[AIdeniyi](https://x.com/Aideniyi)**: A virtual version of MystenLabs' co-founder Adeniyi that responds to users' questions about the Sui ecosystem and occasionally gives away rewards to those who interact with him.
- **[SuiTrivia](https://x.com/SuiTrivia)**: An AI agent that asks questions about the Sui ecosystem, challenging users to respond. Those who answer will have a chance to win rewards.  
- **[SuiTipper](https://x.com/SuiTipper)**: An infrastructure AI agent that handles the financial transactions for the above two agents, managing Sui wallets for X users and AI agents by GiftDrop xWallet.

Additionally, we have developed various infrastructure components related to AI agents, including:  
- [A documentation scraper and database for the Sui ecosystem](https://github.com/SuiMate-AI/docs_scraper), which updates daily by collecting thousands of relevant data points. This enables AI Agents to perform Retrieval-Augmented Generation (RAG) to generate high-quality content. We plan to open access to this via an API.
- [GiftDrop xWallet](https://giftdrop.io/xwallet): A easy-to-connect third party custody wallet solution for X AI agents and users.

We will explain each of the infra and agents in detail in the following sections:


## Infrastructure:
### Sui Ecosystem Database

1. [Our script uses a snowballing method to scrape a large number of documents from the Sui Ecosystem.](https://github.com/SuiMate-AI/docs_scraper/blob/main/1.1_scrape_all.py)  

2. [Next, we use the `intfloat/multilingual-e5-large-instruct` model provided by Atoma to generate embeddings for entire articles.](https://github.com/SuiMate-AI/docs_scraper/blob/main/1.2_embedding.py)  
   - After testing, we found that using whole articles resulted in content being too long, causing some parts to be truncated. So, we decided to split the content into smaller segments.  

3. [By splitting the content into smaller fragments before generating embeddings, we index a single document as multiple vectors, improving search effectiveness.](https://github.com/SuiMate-AI/docs_scraper/blob/main/1.3_embedding_paragraph.py)  

4. [Finally, we upload the embeddings to the Qdrant database.](https://github.com/SuiMate-AI/docs_scraper/blob/main/1.5_upload.py)

Then, by integrating AIdeniyi's reasoning engine powered by Deepseek and LLAMA from Atoma, we further enhance the effectiveness of this search database. Basically, after the user query, the AIdeniyi reasoning engine will first use LLAMA to elaborate on what knowledge point might be related, then use the knowledge point to form embedding to perform a vector search for the database. Finally, the data will be used as the knowledge base to generate replies by Deepseek. All are using Atoma's cloud framework.

### GiftDrop xWallet

<img width="1474" alt="image" src="https://github.com/user-attachments/assets/b2d3ee06-e28c-4c80-a729-4c9ff9535b10" />


We use GiftDrop, a giveaway protocol on Sui, to manage these assets. GiftDrop provides a user-friendly interface for easy management, as shown in the link above: [[xWallet Link](https://giftdrop.io/xwallet)]. AI Agent developers only need to prepare an API Key to connect with GiftDrop's xWallet service, enabling users to manage their assets and interactions with the Agent through the GiftDrop interface.












## AIdeniyi



## Sui Tipper

<img width="696" alt="image" src="https://github.com/user-attachments/assets/24bb8758-738d-4786-9b8a-6fd7224cd024" />


## Sui Trivia

<img width="783" alt="image" src="https://github.com/user-attachments/assets/2b293712-daf6-4f7f-914e-fd5d97215347" />


