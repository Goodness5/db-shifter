# **db-shifter**  

**_Because someone switched your f**king DB URL again._**

---

### 👶 Did your intern point production to the wrong DB?  

### 🤡 Did your devops guy swear "nothing changed" before disappearing?  

### 🔥 Did your CTO say “just restore from backup” like you weren’t already crying?  

Yeah. We’ve all been there.

Welcome to **`db-shifter`** — the little script that digs through your old PostgreSQL database and copies only the missing rows into your new one.

No overwrites. No dumbass `pg_dump`. Just cold, calculated migration for the chaotic neutral in you.

---

## ⚡ Why?

Because your CTO is a clown.  
Because your devops team “accidentally” pointed production at the wrong motherf**king database.  
Because you need to **copy missing shit table-by-table** and you’re too pretty to do it manually.

---

## 🧰 Features

- ✅ Auto-detects all tables in `public` schema  
- ✅ Finds primary keys like a bloodhound  
- ✅ Copies only rows **missing** in the new DB  
- ✅ Skips duplicates (doesn't ruin your existing data)  
- ✅ **Column-based sync** — specify exactly which columns to sync
- ✅ **Circular FK handling** — automatically detects and handles circular foreign key dependencies
- ✅ Smart sync order optimization based on FK relationships
- ✅ FK errors? Nah — this ain't your grandma's `pg_dump`

---

## 💾 Installation

```bash
pip install db-shifter
```

Or if you're a real one:

```bash
git clone https://github.com/goodness5/db-shifter.git
cd db-shifter
pip install -e .
```

---

## 🚀 Usage

### Basic Usage
```bash
db-shifter --old-db-url postgresql://user:pass@oldhost/db --new-db-url postgresql://user:pass@newhost/db
```

### Column-based Sync
Sync only specific columns (useful when schemas differ):
```bash
db-shifter --old-db-url postgresql://user:pass@oldhost/db \
           --new-db-url postgresql://user:pass@newhost/db \
           --columns id,name,email,created_at
```

### Single Table Sync
```bash
db-shifter --old-db-url postgresql://user:pass@oldhost/db \
           --new-db-url postgresql://user:pass@newhost/db \
           --table users
```

### Deep Check Mode
Compare and update column values for existing rows (not just insert missing rows):
```bash
db-shifter --old-db-url postgresql://user:pass@oldhost/db \
           --new-db-url postgresql://user:pass@newhost/db \
           --deep-check
```

This will:
- Insert missing rows (as usual)
- Compare column values for rows that exist in both databases
- Update rows where column values differ

---

## 🧨 Command-line options

| Flag              | What it does                          |
|------------------|----------------------------------------|
| `--dry-run`       | Simulate the transfer, no data is hurt |
| `--verbose`       | Prints detailed logs of every row      |
| `--table users`   | Sync just one table |
| `--skip-fk`       | Ignores foreign key errors             |
| `--columns id,name,email` | Sync only specified columns (comma-separated). Primary key is always included. |
| `--deep-check`    | Deep check: compare column values for existing rows and update differences |

---

## 🧠 How It Works

1. Connects to both DBs  
2. Lists all public tables  
3. Checks the primary key (like a snitch)  
4. Pulls rows missing from the new DB  
5. Inserts them without wrecking existing rows

---

## ⚠️ Caution

- Assumes **you have primary keys** (don't be a barbarian)  
- Circular FK dependencies are now detected and handled, but you may need to manually re-add FK constraints after sync
- If you're syncing 50GB of trash, don't cry when it lags  
- Backups are your friend. Don't be a dumbass.

---

## 🔄 Circular Foreign Key Handling

When `db-shifter` detects circular foreign key dependencies (e.g., Table A → Table B → Table A), it will:

1. **Detect the cycle** and warn you about it
2. **Temporarily disable FK constraints** for tables in the cycle during sync
3. **Sync all data** without FK constraint violations
4. **Note**: You may need to manually re-add FK constraints after sync completes

Example output:
```
⚠️  Circular foreign key dependencies detected:
   Cycle 1: users → profiles → users
   🔄 Will sync these tables in multiple passes with FK constraint handling
```

---

## ✨ Coming Soon

- Timestamp-based syncing (`created_at` support)  
- GUI with a "FIX EVERYTHING" button (for product managers lol)

---

## 🪦 Contributing

Found a bug? Good.  
Fix it, submit a PR, and don't drop your cashapp in the description.

---

## 📜 License

MIT. Do whatever the f**k you want. Just don’t call me if you drop prod again.
