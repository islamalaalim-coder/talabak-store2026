import express from "express";
import cors from "cors";
import dotenv from "dotenv";
import Database from "better-sqlite3";
import path from "path";
import { fileURLToPath } from "url";

dotenv.config();

const __dirname = path.dirname(fileURLToPath(import.meta.url));
const app = express();
const db = new Database("talabak.db");
const PORT = process.env.PORT || 3000;

app.use(cors());
app.use(express.json());
app.use(express.static(path.join(__dirname, "public")));

db.exec(`
CREATE TABLE IF NOT EXISTS products (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  name TEXT NOT NULL,
  price INTEGER NOT NULL,
  stock INTEGER DEFAULT 0,
  category TEXT DEFAULT 'عام',
  image TEXT DEFAULT ''
);
`);

app.get("/api/health", (req, res) => {
  res.json({ ok: true, name: "طلبك" });
});

app.get("/api/products", (req, res) => {
  const q = "%" + (req.query.q || "") + "%";
  const products = db.prepare(
    "SELECT * FROM products WHERE name LIKE ? OR category LIKE ? ORDER BY id DESC"
  ).all(q, q);

  res.json(products);
});

app.get("*", (req, res) => {
  res.sendFile(path.join(__dirname, "public", "index.html"));
});

app.listen(PORT, () => {
  console.log("طلبك يعمل على المنفذ " + PORT);
});
