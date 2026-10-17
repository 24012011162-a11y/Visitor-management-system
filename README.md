# 🏢 Visitor Management System

A role-based web application for registering, approving, and tracking visitors at offices, colleges, residential societies, and corporate buildings. It replaces paper logbooks with a searchable digital record, printable QR passes, live visitor tracking, and exportable reports.

![Python](https://img.shields.io/badge/Python-3.10+-3776AB?logo=python&logoColor=white)
![Flask](https://img.shields.io/badge/Flask-3.x-000000?logo=flask&logoColor=white)
![SQLite](https://img.shields.io/badge/Database-SQLite-003B57?logo=sqlite&logoColor=white)
![Bootstrap](https://img.shields.io/badge/UI-Bootstrap_5-7952B3?logo=bootstrap&logoColor=white)
![License](https://img.shields.io/badge/license-MIT-blue)

<!-- Add screenshots to docs/screenshots/ and uncomment:
![Dashboard](docs/screenshots/dashboard.png)
![Visitor Pass](docs/screenshots/pass.png)
-->

---

## ✨ Features

**Visitor handling**
- Visitor registration with host, appointment, ID proof, and optional photo upload
- Auto-generated unique visitor IDs (e.g. `VIS-2026-00001`)
- Approval workflow: Pending → Approved / Rejected / Cancelled
- Check-in and check-out with timestamps, visit duration, and activity history
- Printable visitor pass with QR code
- Live visitor log that auto-refreshes every 12 seconds

**Security & control**
- Blacklist management, with check-in blocked unless an Admin overrides
- Role-based access for Admin, Security, and Receptionist
- CSRF protection, hashed passwords, validated image uploads, secure session cookies, and friendly 403/404/500 pages

**Management & insights**
- Dashboard with live statistics and Chart.js charts
- Host management (add, edit, deactivate, search, view associated visitors)
- Daily, weekly, and monthly reports, plus host-wise and department-wise breakdowns
- Export to CSV or PDF, or print from the browser
- In-app notifications with unread count, plus optional email via SMTP
- Admin user management: roles, deactivation, password reset

## 🛠️ Tech Stack

| Layer | Technology |
|-------|-----------|
| Backend | Python, Flask |
| Database | SQLite via Flask-SQLAlchemy (ORM) |
| Auth & security | Flask-Login, Werkzeug password hashing, Flask-WTF (CSRF) |
| Frontend | HTML5, CSS3, JavaScript, Bootstrap 5, Bootstrap Icons, Chart.js |
| Extras | Flask-Mail, qrcode, Pillow, ReportLab |
| Testing | pytest |

## 👥 Roles & Permissions

| Capability | Admin | Security | Receptionist |
|-----------|:-----:|:--------:|:------------:|
| Register visitors | ✅ | ✅ (auto-approved) | ✅ (pending approval) |
| Edit visitors, approve / reject / cancel | ✅ | ❌ | ✅ |
| Check in / check out | ✅ | ✅ | ❌ |
| Override blacklist at check-in | ✅ | ❌ | ❌ |
| Add / edit hosts | ✅ | ❌ | ✅ |
| Send notifications | ✅ | ❌ | ✅ |
| Reports and exports | ✅ | ❌ | ❌ |
| User management, deletions | ✅ | ❌ | ❌ |

## 🚀 Getting Started

### Prerequisites
- Python 3.10 or higher
- Git

### Installation

```bash
# 1. Clone the repository
git clone https://github.com/your-username/visitor-management-system.git
cd visitor-management-system

# 2. Create and activate a virtual environment
python -m venv venv
venv\Scripts\activate          # Windows
# source venv/bin/activate     # macOS / Linux

# 3. Install dependencies
pip install -r requirements.txt

# 4. Initialize the database with demo data
python seed.py

# 5. Run the app
python app.py
```

Open **http://127.0.0.1:5000** in your browser.

> **Windows shortcut:** double-click `run_windows.bat` to do all of the above in one step.

### Demo Credentials

| Role | Email | Password |
|------|-------|----------|
| Admin | `admin@visitor.com` | `Admin@123` |
| Security | `security1@visitor.com` | `Security@123` |
| Receptionist | `reception1@visitor.com` | `Reception@123` |

Two more demo accounts exist (`security2@visitor.com`, `reception2@visitor.com`) with the same password pattern.

> ⚠️ These are for development only. Change every password before any real deployment.

## ⚙️ Configuration

Copy `.env.example` to `.env` and adjust the values. All variables are optional for local use.

| Variable | Purpose | Default |
|----------|---------|---------|
| `SECRET_KEY` | Flask session signing key | dev placeholder (**change in production**) |
| `DATABASE_URL` | Database connection string | `sqlite:///instance/database.db` |
| `FLASK_HOST` / `FLASK_PORT` | Server address | `127.0.0.1` / `5000` |
| `FLASK_DEBUG` | Debug mode | `false` |
| `SESSION_COOKIE_SECURE` | Set `true` when serving over HTTPS | `false` |
| `MAX_UPLOAD_BYTES` | Max photo upload size | 2 MB |
| `MAIL_SERVER`, `MAIL_PORT`, `MAIL_USERNAME`, `MAIL_PASSWORD`, `MAIL_USE_TLS`, `MAIL_DEFAULT_SENDER` | Optional SMTP email notifications | unset |

Email is optional. Without SMTP settings, the app uses in-app notifications only.

### Resetting the database

```bash
python seed.py            # create tables and ensure demo data exists
python seed.py --reset    # drop everything and restore demo data
```

## 🔄 Typical Workflow

1. A **Receptionist** (or Admin) registers the visitor with host, purpose, and ID details
2. The request is **approved** or rejected (Security-registered visitors are auto-approved for walk-ins)
3. **Security** checks the visitor in. The system verifies blacklist status first
4. A **QR visitor pass** is printed
5. On exit, Security **checks the visitor out** and the visit duration is stored
6. **Admin** reviews dashboards and exports reports

## 📂 Project Structure

```text
visitor-management-system/
├── app.py              # App factory, seeding, error handlers
├── config.py           # Environment-based configuration
├── seed.py             # Database init / reset script
├── tests.py            # pytest test suite
├── requirements.txt
├── .env.example
├── models/             # User, Visitor, Host, Notification, Blacklist
├── routes/             # auth, dashboard, visitors, hosts, notifications, reports
├── templates/          # Jinja2 templates (visitors, hosts, users, reports, errors)
└── static/             # CSS, JavaScript, uploaded photos
```

## 🧪 Testing

```bash
python -m pytest -q
```

Tests cover login and invalid login, role restrictions, the approval and check-in/out flow, blacklist enforcement, and CSV/PDF exports.

## 🌐 Deployment

Don't use the Flask development server in production. Use a production WSGI server:

```bash
# Windows
pip install waitress
waitress-serve --listen=127.0.0.1:5000 app:app

# Linux
pip install gunicorn
gunicorn app:app
```

Before going live:
- Set a strong `SECRET_KEY`
- Serve over HTTPS and set `SESSION_COOKIE_SECURE=true`
- Replace all demo credentials
- Consider a production database (e.g. PostgreSQL) via `DATABASE_URL`
- Keep SMTP and database credentials in environment variables, never in source control

## 🗺️ Roadmap

- [ ] SMS / WhatsApp notifications
- [ ] Visitor self check-in kiosk using the QR code
- [ ] Pre-registration link for expected guests
- [ ] Docker support
- [ ] PostgreSQL migration guide

## 🤝 Contributing

Contributions are welcome.

1. Fork the repository
2. Create a branch: `git checkout -b feature/your-feature`
3. Commit your changes: `git commit -m "Add your feature"`
4. Push: `git push origin feature/your-feature`
5. Open a Pull Request


## 👤 Author

**Priyanshu Raj**
- GitHub: [@24012011162-a11y](https://github.com/24012011162-a11y)
- LinkedIn: [Priyanshu Raj](https://linkedin.com/in/priyanshu-raj-b2ba91397)

---

⭐ If you find this project useful, please consider giving it a star.
