# Ironlight — Technical Assessment

**Developer:** Zulkifli Raihan
**LinkedIn:** [linkedin.com/in/zulkifliraihan](https://www.linkedin.com/in/zulkifliraihan)
**GitHub:** [github.com/zulkiflirain](https://github.com/zulkiflirain)

---

> **Disclaimer:** This project was built solely as a technical assessment submission. It is not intended for public deployment, commercial use, or production environments. All content, design, and implementation are for evaluation purposes only.

---

## Requirements

| Dependency | Version Used | Minimum |
|---|---|---|
| PHP | 8.3 | 8.0+ |
| Composer | 2.8 | 2.0+ |
| Node.js | 24.5 | 18.x+ |
| SQLite | bundled (`database/database.sqlite`) | 3.x |

> No external database server is required. The project uses SQLite out of the box.

---

## Installation

```bash
# 1. Clone the repository
git clone <repository-url>
cd <project-folder>

# 2. Install PHP dependencies
composer install

# 3. Install Node.js dependencies
npm install

# 4. Copy environment file and generate application key
cp .env.example .env
php artisan key:generate

# 5. Run migrations and seed default data
php artisan migrate --seed

# 6. Link storage for uploaded assets
php artisan storage:link

# 7. Build frontend assets
npm run build

# 8. Start the development server
php artisan serve
```

The application will be available at `http://localhost:8000`.

> **For active development**, run `npm run dev` in a separate terminal instead of `npm run build` to get hot module replacement.

---

## Access

| Route | Description |
|---|---|
| `/` | Public landing page |
| `/admin` | Admin dashboard (Filament) |

**Admin credentials (seeded):**

| Field | Value |
|---|---|
| Email | zuran2907@gmail.com |
| Password | 123123123 |

---

## Architectural Decisions

### Landing Page — Route → Controller → Blade

The landing page follows a deliberately simple, three-layer architecture: a single route resolves to `HomeController`, which fetches all CMS content from the database in one query and passes it to the Blade view.

A more layered pattern — such as a module structure, service classes, or a repository layer — was intentionally avoided. The landing page is a single, read-only page with no business logic beyond data retrieval. Introducing those abstractions would add structural overhead without any meaningful benefit for a task of this scope. Keeping it flat makes the data flow immediately obvious and the code easy to follow.

### Admin Dashboard — Filament

The admin panel is built on [Filament](https://filamentphp.com/), a first-party Laravel admin framework. Rather than hand-crafting CRUD pages, controllers, and form validation from scratch, Filament handles all of that through declarative configuration — defining the table columns, form fields, and filters is all that is needed to get a fully functional interface.

This was the right call for a time-boxed assessment: it allows the CMS to be fully functional and polished without diverting effort away from the core deliverable. The result is the same production-quality interface a custom-built panel would produce, in a fraction of the time.

### CMS — Dynamic Key-Value Schema

Page content is stored in a single `page_settings` table using a `key | value | type` structure rather than a fixed-column model. This means the CMS can accommodate any new content field — text, image, JSON, rich text — without requiring a schema migration. The `type` column drives how Filament renders the edit field and how the value is decoded when passed to the view.
