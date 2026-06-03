# DrakeWater Supply Management System

A comprehensive water supply management system built with Django backend and Vue.js frontend.

## Features

1. ✅ CRUD operations for all models
2. ✅ Admin login integration
3. ✅ Image handling for customers and meters
4. ✅ Photo album functionality
5. ✅ User-based data filtering
6. ✅ Data generation scripts (1000+ records)
7. ✅ Statistics and aggregation queries
8. ✅ Two-factor authentication (2FA)
9. ✅ Column-based filtering
10. ✅ User store and authentication
11. ✅ Data export to Excel/Word

## Project Structure

```
drakewater_supply/
├── manage.py
├── requirements.txt
├── README.md
├── drakewater_supply/
│   ├── settings.py
│   ├── urls.py
│   ├── asgi.py
│   └── wsgi.py
├── water_supply/
│   ├── models.py (Customer, Meter, Bill, MeterReading, etc.)
│   ├── serializers.py
│   ├── views.py (API endpoints with 2FA, stats, export)
│   ├── urls.py
│   ├── admin.py
│   └── management/
│       └── commands/
│           └── generate_data.py
├── media/ (uploaded images)
└── client/
    ├── src/
    │   ├── components/
    │   ├── views/
    │   ├── store/
    │   ├── router.js
    │   ├── main.js
    │   └── App.vue
    └── vite.config.js
```

## Backend Setup

### 1. Create virtual environment
```bash
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
```

### 2. Install dependencies
```bash
pip install -r requirements.txt
```

### 3. Run migrations
```bash
python manage.py makemigrations
python manage.py migrate
```

### 4. Create superuser
```bash
python manage.py createsuperuser
# Username: admin
# Password: admin123
```

### 5. Generate sample data (1000+ records)
```bash
python manage.py generate_data 200
```

### 6. Run Django server
```bash
python manage.py runserver
# Server runs on http://localhost:8000
# Admin: http://localhost:8000/admin
```

## Frontend Setup

### 1. Navigate to client folder
```bash
cd client
```

### 2. Install dependencies
```bash
npm install
```

### 3. Run development server
```bash
npm run dev
# Frontend runs on http://localhost:5173
```

## Models

### District
- name: CharField
- description: TextField
- user: ForeignKey (User)

### Customer
- name: CharField
- email: EmailField (unique)
- phone: CharField
- address: TextField
- customer_type: Choice (residential, commercial, industrial)
- picture: ImageField
- district: ForeignKey (District)
- user: ForeignKey (User)
- is_active: BooleanField

### Meter
- meter_number: CharField (unique)
- customer: ForeignKey (Customer)
- current_reading: DecimalField
- picture: ImageField
- status: Choice (active, inactive, broken)
- installation_date: DateField
- user: ForeignKey (User)

### MeterReading
- meter: ForeignKey (Meter)
- reading: DecimalField
- reading_date: DateField
- consumption: DecimalField (auto-calculated)
- user: ForeignKey (User)

### Bill
- customer: ForeignKey (Customer)
- meter_reading: ForeignKey (MeterReading)
- amount: DecimalField
- status: Choice (pending, paid, overdue)
- issue_date: DateField
- due_date: DateField
- payment_date: DateField (nullable)
- user: ForeignKey (User)

### PhotoAlbum
- name: CharField
- description: TextField
- customer: ForeignKey (Customer, nullable)
- meter: ForeignKey (Meter, nullable)
- user: ForeignKey (User)

### AlbumPhoto
- album: ForeignKey (PhotoAlbum)
- photo: ImageField
- caption: CharField
- uploaded_at: DateTimeField

### UserProfile
- user: OneToOneField (User)
- opt_key: CharField (for 2FA)
- two_fa_enabled: BooleanField

## API Endpoints

### Districts
- `GET /api/districts/` - List districts
- `POST /api/districts/` - Create district
- `GET /api/districts/{id}/` - Get district
- `PUT /api/districts/{id}/` - Update district
- `DELETE /api/districts/{id}/` - Delete district
- `GET /api/districts/stats/` - Get statistics

### Customers
- `GET /api/customers/` - List customers (with filtering)
- `POST /api/customers/` - Create customer
- `GET /api/customers/{id}/` - Get customer
- `PUT /api/customers/{id}/` - Update customer
- `DELETE /api/customers/{id}/` - Delete customer
- `GET /api/customers/stats/` - Get statistics
- `GET /api/customers/export_excel/` - Export to Excel

### Meters
- `GET /api/meters/` - List meters (with filtering)
- `POST /api/meters/` - Create meter
- `GET /api/meters/{id}/` - Get meter
- `PUT /api/meters/{id}/` - Update meter (requires 2FA)
- `DELETE /api/meters/{id}/` - Delete meter
- `GET /api/meters/stats/` - Get statistics

### Bills
- `GET /api/bills/` - List bills (with filtering)
- `POST /api/bills/` - Create bill
- `GET /api/bills/{id}/` - Get bill
- `PUT /api/bills/{id}/` - Update bill
- `DELETE /api/bills/{id}/` - Delete bill
- `GET /api/bills/stats/` - Get statistics

### Readings
- `GET /api/readings/` - List readings
- `POST /api/readings/` - Create reading
- `GET /api/readings/stats/` - Get statistics

### Albums
- `GET /api/albums/` - List albums
- `POST /api/albums/` - Create album
- `GET /api/albums/{id}/` - Get album
- `POST /api/albums/{id}/add_photo/` - Add photo to album

### Authentication & 2FA
- `GET /api/profile/check-login/` - Check if user is authenticated
- `GET /api/profile/current-user/` - Get current user info
- `POST /api/profile/setup-2fa/` - Setup 2FA (returns secret and QR code URI)
- `POST /api/profile/enable-2fa/` - Enable 2FA with OTP code
- `POST /api/profile/otp-login/` - Login with OTP code
- `GET /api/profile/otp-status/` - Check 2FA status

## Features Implemented

### 1. CRUD Operations ✅
- Complete CRUD for all models via REST API
- Vue.js components for managing each model

### 2. Admin Login ✅
- Integrated admin login button in Vue frontend
- Links to Django admin panel

### 3. Image Handling ✅
- Picture fields on Customer and Meter models
- Upload images when creating/editing records
- Modal popup to view images in detail

### 4. Photo Albums ✅
- Create albums for customers/meters
- Upload multiple photos to albums
- Gallery view to browse photos

### 5. User-Based Data ✅
- All data tied to authenticated user
- Superuser can see all data
- Regular users see only their own data
- Optional: User filter in admin for superuser

### 6. Data Generation ✅
- `generate_data` management command
- Faker library for realistic data
- Generates 200+ customers with meters, readings, and bills

### 7. Statistics ✅
- `/stats/` endpoints for each model
- Aggregation queries (Count, Sum, Avg, Max, Min)
- Statistics displayed alongside data tables

### 8. Two-Factor Authentication ✅
- Setup 2FA with QR code
- OTP verification with pyotp
- 10-minute session cache for authenticated actions
- Edit operations require 2FA

### 9. Filtering ✅
- Filter by customer_type, status, district
- Multiple column filters
- Query parameter-based filtering

### 10. User Store & Auth ✅
- Vuex/Pinia store for user state
- Login/logout functionality
- Protected routes
- Current user display

### 11. Data Export ✅
- Export customers to Excel format
- Uses openpyxl library
- Separate action endpoint

## Usage Examples

### Generate Data
```bash
python manage.py generate_data 500  # Creates 500 customers with related data
```

### Access Admin
```
http://localhost:8000/admin
Username: admin
Password: admin123
```

### API Usage
```bash
# List customers
curl -H "X-CSRFToken: {token}" http://localhost:8000/api/customers/

# Get stats
curl http://localhost:8000/api/customers/stats/

# Export to Excel
curl http://localhost:8000/api/customers/export_excel/ --output customers.xlsx

# Setup 2FA
curl -X POST http://localhost:8000/api/profile/setup-2fa/

# Enable 2FA
curl -X POST http://localhost:8000/api/profile/enable-2fa/ \
  -H "Content-Type: application/json" \
  -d '{"key": "123456"}'

# Edit meter (requires 2FA)
curl -X PUT http://localhost:8000/api/meters/1/ \
  -H "Content-Type: application/json" \
  -d '{"status": "inactive"}'
```

## Technologies Used

### Backend
- Django 4.2
- Django REST Framework 3.14
- Pillow (image handling)
- Faker (data generation)
- PyOTP (2FA)
- openpyxl (Excel export)
- python-docx (Word export)
- django-redis (caching)
- django-cors-headers (CORS support)

### Frontend
- Vue.js 3
- Bootstrap 5
- Axios (HTTP client)
- Vue Router (routing)
- Pinia/Vuex (state management)

## Next Steps

1. ✅ Set up Django backend
2. ✅ Create Vue.js frontend components
3. ✅ Implement all 11 features
4. Run data generation
5. Test all endpoints
6. Deploy to production

## Support

For issues or questions, check the tutorial documentation in the project root.
