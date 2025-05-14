# Mini YouTube MongoDB Dataset

Course exercise • **_no JSON Schema validators applied_**

---

## 1  Overview
This dataset models a reduced version of YouTube for classroom practice.  
Import it into any MongoDB instance and explore CRUD operations, aggregation
pipelines and basic data‑modelling patterns without worrying about schema
validation.

---

## 2  Folder structure
```
.
├── seed‑YouTube.js          # one–shot script (mongosh) that inserts every document
└── json‑samples/            # same data split in 7 JSON files – handy for Compass
    ├── users.json
    ├── channels.json
    ├── videos.json
    ├── subscriptions.json
    ├── videoVotes.json
    ├── playlists.json
    └── comments.json
```
*Choose **either** the `seed‑YouTube.js` script **or** the individual JSON files
– not both.*

---

## 3  Collections & fields
| Collection | What it stores | Key fields |
|------------|----------------|------------|
| **users** | basic account details | `email`, `password`, `username`, `birthDate`, … |
| **channels** | one channel per user | `ownerId` → `users._id`, `name`, `description` |
| **videos** | all uploaded videos | `uploaderId`, `title`, `tags`, `state`, counters |
| **subscriptions** | who follows which channel | `subscriberId`, `channelId`, `subscribedAt` |
| **videoVotes** | likes / dislikes | `videoId`, `userId`, `vote` (`like` / `dislike`) |
| **playlists** | user‑defined video lists | `ownerId`, `name`, `visibility`, embedded `videos[]` |
| **comments** | text threads under videos | `videoId`, `userId`, `text`, `createdAt` |

> No JSON Schema validators are applied, so you are free to insert or update
> documents with any shape while experimenting.

---

## 4  Requirements
* MongoDB 6 .x or later (Atlas cluster, Docker container or local installation)  
* MongoDB Compass ≥ 1.32 (for graphical import) **or** `mongosh` (CLI)

---

## 5  Quick start

### 5.1 Import with Compass
1. Open Compass and select / create a database, e.g. **Youtube**.  
2. For each collection listed above:  
   1. Create the collection (no additional options).  
   2. **Add Data → Import File → JSON**.  
   3. Choose the corresponding file inside `json‑samples/`.  
   4. **Import as:** **Extended JSON (v2)**, then *Import*.  
3. Repeat until all seven collections are filled.

### 5.2 Import with `mongosh`
```bash
mongosh "mongodb://localhost:27017" seed‑YouTube.js
```
The script switches to (or creates) a **Youtube** database and performs one
`insertMany()` per collection.

---

## 6  Things to try
| Idea | Example command |
|------|-----------------|
| List public “cooking” videos | `db.videos.find({ state: "public", tags: "cooking" })` |
| Count subscribers per channel | `$group` on `subscriptions` |
| Show last 5 comments on a video | `db.comments.find({ videoId: … }).sort({ createdAt: -1 }).limit(5)` |
| Build “top liked videos” view | `$lookup` `videoVotes`→`videos`, `$match:{ vote:"like" }`, `$group` |

*(Because no validators are active you can also practise “bad inserts” and then
clean them up.)*

---

## 7  Troubleshooting
* **Import error “string is not of type objectId”** – ensure **Extended JSON (v2)**
  is selected; legacy mode strips `$oid`.
* **Duplicate key error** – if you imported twice, drop the collection or remove
  existing documents before retrying.
* Still stuck? Open an issue in your class repo.

---

## 8  License / credits
Created for educational purposes in the **MongoDB Exercises** course.  
Feel free to fork, adapt and extend — just keep this README reference.
