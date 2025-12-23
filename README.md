# Library Management System

A Django-based library management system for managing books, authors, members, and borrowing records.

## Features

### Books Management
- Maintain a catalog of books with ISBN, title, authors, publisher, and publication details
- Track available and total copies of each book
- Categorize books by genre/category
- Support multiple authors per book

### Author Management
- Store author information including biography and birth date
- Link authors to their published books
- Contact information for authors

### Member Management
- Register and manage library members
- Track membership status (active, inactive, suspended)
- Set membership expiry dates
- Control maximum books allowed per member
- Track member contact and address information

### Borrowing System
- Track book borrowing and returns
- Automatic due date calculation (14 days from borrow date)
- Monitor overdue books
- Calculate days overdue for tracking penalties

### Fine Management
- Track fines for overdue books
- Monitor payment status
- Record payment dates

## Project Structure

```
library_system/
├── library_system/          # Main project settings
│   ├── settings.py          # Django settings
│   ├── urls.py              # URL configuration
│   ├── asgi.py
│   └── wsgi.py
├── books/                   # Books and borrowing app
│   ├── models.py            # Book, Author, Borrowing models
│   ├── admin.py             # Admin interface configuration
│   ├── views.py
│   ├── urls.py
│   └── migrations/          # Database migrations
├── members/                 # Members and fines app
│   ├── models.py            # Member and Fine models
│   ├── admin.py             # Admin interface configuration
│   ├── views.py
│   ├── urls.py
│   └── migrations/          # Database migrations
├── manage.py                # Django management script
├── db.sqlite3               # SQLite database (development)
└── requirements.txt         # Python dependencies
```

## Installation

### Prerequisites
- Python 3.8 or higher
- pip (Python package manager)

### Setup Steps

1. **Create a virtual environment** (optional but recommended):
   ```powershell
   python -m venv .venv
   .venv\Scripts\activate
   ```

2. **Install dependencies**:
   ```powershell
   pip install Django==4.2.10 python-dateutil
   ```

3. **Apply database migrations**:
   ```powershell
   python manage.py migrate
   ```

4. **Create a superuser account**:
   ```powershell
   python manage.py createsuperuser
   ```

5. **Start the development server**:
   ```powershell
   python manage.py runserver
   ```

6. **Access the admin interface**:
   - Open your browser and navigate to `http://127.0.0.1:8000/admin/`
   - Log in with the superuser credentials you created

## Database Models

### Author
- `first_name`: Author's first name
- `last_name`: Author's last name
- `email`: Contact email (optional)
- `biography`: Author biography (optional)
- `birth_date`: Author's birth date (optional)
- `created_at`: Timestamp when created
- `updated_at`: Timestamp when last updated

### Book
- `ISBN`: Unique ISBN-13 identifier
- `title`: Book title
- `authors`: Many-to-many relationship with Author
- `publisher`: Publishing company
- `publication_date`: Date of publication (optional)
- `pages`: Number of pages
- `language`: Language of the book (default: English)
- `description`: Book description
- `category`: Genre/category classification
- `available_copies`: Current number of available copies
- `total_copies`: Total copies in the library
- `created_at`: Timestamp when created
- `updated_at`: Timestamp when last updated

### Member
- `user`: One-to-one relationship with Django User
- `membership_id`: Unique member identifier
- `phone_number`: Contact phone number
- `address`: Member's address
- `city`: City name
- `postal_code`: Postal code
- `country`: Country name
- `membership_date`: Date membership started
- `expiry_date`: Date membership expires (auto-set to 1 year)
- `status`: Membership status (active, inactive, suspended)
- `max_books_allowed`: Maximum books member can borrow (default: 5)
- `created_at`: Timestamp when created
- `updated_at`: Timestamp when last updated

### Borrowing
- `book`: Foreign key to Book
- `member`: Foreign key to Member
- `borrow_date`: When the book was borrowed
- `due_date`: When the book is due (auto-set to 14 days)
- `return_date`: When the book was returned (optional)
- `is_returned`: Whether the book has been returned
- Methods:
  - `is_overdue()`: Check if book is overdue
  - `days_overdue()`: Get number of days overdue

### Fine
- `member`: Foreign key to Member
- `borrowing`: One-to-one relationship with Borrowing (optional)
- `amount`: Fine amount
- `reason`: Reason for the fine
- `issue_date`: Date fine was issued
- `due_date`: Payment due date
- `payment_date`: Date fine was paid (optional)
- `status`: Payment status (unpaid, paid)
- Methods:
  - `is_overdue()`: Check if fine payment is overdue

## Usage Examples

### Via Admin Interface

1. **Add an Author**:
   - Navigate to Authors in admin
   - Click "Add Author"
   - Enter first name, last name, and optional details
   - Save

2. **Add a Book**:
   - Navigate to Books in admin
   - Click "Add Book"
   - Enter book details including ISBN, title, authors
   - Set available and total copies
   - Save

3. **Register a Member**:
   - Navigate to Members in admin
   - Click "Add Member"
   - Link to a User account
   - Enter membership ID and member details
   - Save (expiry date auto-sets to 1 year)

4. **Create a Borrowing Record**:
   - Navigate to Borrowings in admin
   - Click "Add Borrowing"
   - Select book and member
   - Save (due date auto-sets to 14 days)

5. **Record a Return**:
   - Edit the Borrowing record
   - Set return date and check "is_returned"
   - Save

6. **Issue a Fine**:
   - Navigate to Fines in admin
   - Click "Add Fine"
   - Select member, amount, and reason
   - Set payment due date
   - Save

## Admin Features

- **Customized list displays** for quick overview of data
- **Search functionality** for finding records quickly
- **Filters** for sorting by status, dates, etc.
- **Read-only fields** for calculated values
- **Field grouping** with collapsible sections

## Next Steps

To extend this project, you can:

1. **Create Views and Templates**:
   - Build views for browsing books and members
   - Create forms for borrowing and returning books
   - Build member dashboard

2. **Add API Endpoints**:
   - Use Django REST Framework for API
   - Create endpoints for books, members, borrowing

3. **Add Authentication**:
   - Implement member login
   - Create permission-based access control

4. **Send Notifications**:
   - Email notifications for due dates
   - SMS reminders for overdue books

5. **Generate Reports**:
   - Borrowing statistics
   - Popular books report
   - Member activity report

6. **Automated Tasks**:
   - Use Celery for background tasks
   - Auto-generate fines for overdue books
   - Send reminder emails

## Development

### Running Tests
```powershell
python manage.py test
```

### Creating New Migrations
```powershell
python manage.py makemigrations
python manage.py migrate
```

### Database Shell
```powershell
python manage.py dbshell
```

### Django Shell
```powershell
python manage.py shell
```

## Settings

Key Django settings in `library_system/settings.py`:
- `DEBUG = True` - For development (change to `False` for production)
- `INSTALLED_APPS` - Includes books and members apps
- `DATABASES` - SQLite for development (use PostgreSQL for production)

## Troubleshooting

### Port Already in Use
If port 8000 is already in use:
```powershell
python manage.py runserver 8001
```

### Migration Errors
If you encounter migration issues:
```powershell
python manage.py showmigrations
python manage.py migrate --fake initial
```

## Technology Stack

- **Framework**: Django 4.2.10
- **Database**: SQLite (development) / PostgreSQL (recommended for production)
- **Python**: 3.8+
- **Server**: Django development server (for production use Gunicorn/uWSGI)

## License

This project is open source and available under the MIT License.

## Support

For issues or questions, please refer to the Django documentation:
- [Django Documentation](https://docs.djangoproject.com/)
- [Django Admin Documentation](https://docs.djangoproject.com/en/4.2/ref/contrib/admin/)
