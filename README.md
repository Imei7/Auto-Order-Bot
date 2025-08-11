# 🛍️ Modern Telegram Store Bot

A modern, fully automated Telegram store bot with QRIS Saweria payment integration, professional UI, and wizard-based interfaces.

## ✨ Features

### 🚀 **Core Features**
- **Fully Automated Store**: Complete e-commerce solution
- **QRIS Saweria Payment**: Instant payment via all e-wallets
- **Auto Delivery**: Products delivered automatically after payment
- **Professional UI**: Modern interface with loading animations, pagination, and checklists
- **Admin Dashboard**: Comprehensive admin panel with reply keyboards

### 💳 **Payment System**
- **Primary**: QRIS Saweria via Maelyn API
- **Auto Check**: Payment verification every 5 minutes
- **Dual Method**: Image buffer + QR string generation (backup)
- **Instant Delivery**: Products sent automatically upon payment confirmation

### 🎨 **User Interface**
- **Reply Keyboards**: Main navigation with custom buttons
- **Inline Keyboards**: Interactive menus and wizards
- **Professional Pagination**: Clean pagination for large lists
- **Loading Animations**: Visual feedback during operations
- **Step-by-step Wizards**: Guided product creation and purchasing

### 🛡️ **Admin Features**
- **Simplified Interface**: Reply keyboard instead of complex commands
- **Product Wizard**: Step-by-step product creation
- **Stock Management**: Easy stock addition and monitoring
- **User Management**: Complete user control and statistics
- **System Monitoring**: Real-time system status and analytics

## 🚀 **Quick Start**

### 1. **Installation**
```bash
npm install
```

### 2. **Environment Setup**
Create `.env` file:
```env
BOT_TOKEN=your_telegram_bot_token
MAELYN_API_KEY=your_maelyn_api_key_here
SAWERIA_USERNAME=your_saweria_username_here
```

⚠️ **Important**: 
- Get your Maelyn API key from the official provider
- Use your Saweria username (the one in saweria.co/username)
- Ensure both are properly configured for payment processing

### 3. **Configuration**
Edit `resources/Admin/settings.json`:
```json
{
    "owner_chatid": "your_chat_id",
    "store_name": "YOUR_STORE_NAME",
    "owner_usn": "@your_username",
    "saweria_settings": {
        "username": "your_saweria_username"
    }
}
```

### 4. **Run**
```bash
npm start
```

## 📁 **Project Structure**

```
├── commands/
│   ├── admin.js       # Admin interface with reply keyboards
│   ├── start.js       # Modern start menu with dashboard
│   ├── produk.js      # Product management with purchase wizard
│   └── settings.js    # Simplified settings interface
├── resources/
│   ├── payments/
│   │   └── maelynAPI.js    # Saweria QRIS payment integration
│   ├── lib/
│   │   ├── paymentManager.js    # Payment monitoring and auto-delivery
│   │   └── uiHelpers.js         # Professional UI components
│   ├── Admin/
│   │   └── settings.json        # Bot configuration
│   └── database/
│       ├── database.db          # SQLite database
│       ├── userlist.json        # User data
│       └── listTransaksi.json   # Transaction history
├── index.js           # Main bot entry point
└── package.json       # Dependencies
```

## 🔧 **Configuration**

### **Store Settings** (`resources/Admin/settings.json`)
- `store_name`: Your store name
- `owner_chatid`: Admin chat ID
- `owner_usn`: Admin username
- `saweria_settings`: Saweria payment configuration
- `button_menu`: Custom button texts
- `ui_settings`: UI preferences

### **Payment Configuration**
The bot uses **Maelyn API** for Saweria QRIS integration:
- Base URL: `https://api.maelyn.sbs/api`
- Authentication: `mg-apikey` header (not in body)
- Endpoints: `/saweria/check-user`, `/saweria/create-payment`, `/saweria/check-payment`
- Auto-check interval: 5 minutes
- Dual QR method: API image + generated backup

**Payment Setup:**
1. Get your API key from Maelyn provider
2. Create/verify your Saweria account and note your username
3. Add both to `.env`:
   - `MAELYN_API_KEY=your_key_here`
   - `SAWERIA_USERNAME=your_username_here`
4. The system uses `mg-apikey` header format automatically
5. Test connection via Admin Panel → Settings → Payment Config

## 🛍️ **How to Use**

### **For Customers:**
1. `/start` - Open modern dashboard
2. Click "🛍️ List Produk" - Browse products
3. Select product and quantity
4. Enter Saweria username
5. Scan QR code and pay
6. Receive product automatically

### **For Admins:**
1. `/adminmenu` - Open admin panel
2. Use reply keyboard navigation
3. "📦 Produk" - Manage products with wizard
4. "👥 Users" - User management
5. "💰 Payments" - Payment monitoring
6. "📊 Laporan" - System reports

## 📦 **Product Management**

### **Adding Products** (Wizard-based)
1. Admin Panel → 📦 Produk → ➕ Tambah Produk
2. Step 1: Product name
3. Step 2: **Product type** (Digital Text or File)
4. Step 3: Price
5. Step 4: Category
6. Step 5: Description
7. Step 6: Terms & conditions
8. Step 7: Confirmation

### **File Products Management**
- **Upload Files**: Send file with caption `ID: [product_id]`
- **Supported Types**: ZIP, PDF, images, documents, etc.
- **Multiple Files**: Upload several files per product
- **Auto Delivery**: Files sent automatically after payment
- **File Management**: View, delete, and organize product files

### **Stock Management**
- Add individual stock items
- Bulk stock import
- Auto stock tracking
- Expiry date management

## 💳 **Payment Flow**

1. **User initiates purchase**
2. **System checks Saweria username** via Maelyn API
3. **Creates payment** with unique reference
4. **Generates QR code** (dual method)
5. **Monitors payment** every 5 minutes
6. **Auto-delivers product** upon confirmation
7. **Updates database** and sends notifications

## 🎨 **UI Components**

### **Professional Pagination**
```javascript
const pagination = UIHelpers.createPagination(
    items, currentPage, itemsPerPage, 'callback_prefix', itemFormatter
);
```

### **Loading Animations**
```javascript
const loadingMsg = UIHelpers.createLoadingMessage('Loading...', step);
```

### **Progress Indicators**
```javascript
const progress = UIHelpers.createProgressIndicator(
    currentStep, totalSteps, stepNames
);
```

### **Checklists**
```javascript
const checklist = UIHelpers.createChecklist(
    items, checkedItems, 'callback_prefix'
);
```

## 📢 **Broadcast System**

### **Broadcast Types**
- **Text Only**: Send text messages to all users
- **Image + Text**: Send photos with captions to all users
- **Rate Limiting**: Automatic delays to prevent API limits
- **Success Tracking**: Monitor delivery statistics

### **How to Broadcast**
1. Admin Panel → 📢 Broadcast
2. Choose broadcast type (Text or Image+Text)
3. Enter message or send photo with caption
4. Confirm and send to all users

## 🔄 **API Integration**

### **Maelyn API Class**
```javascript
const maelynAPI = new MaelynAPI();

// Check user by username
const userCheck = await maelynAPI.checkUserByUsername('username');

// Create payment
const payment = await maelynAPI.createPayment(userid, amount, reference);

// Check payment status
const status = await maelynAPI.checkPaymentStatus(reference);
```

## 📊 **Database Schema**

### **Products** (`list` table)
- `id`: Product ID
- `nameproduct`: Product name
- `price`: Product price
- `category`: Product category
- `desc`: Description
- `snk`: Terms & conditions

### **Stock** (`stock` table)
- `stock_no`: Stock ID
- `id`: Product ID (foreign key)
- `info`: Stock information/data
- `expired_at`: Expiry date

### **Payments** (`payments` table)
- `reference`: Payment reference
- `chat_id`: User chat ID
- `amount`: Payment amount
- `status`: Payment status
- `order_data`: Order details
- `saweria_username`: Saweria username

## 🚨 **Error Handling**

- **Graceful failure**: All operations have proper error handling
- **User feedback**: Clear error messages and status updates
- **Auto-retry**: Payment checking with configurable intervals
- **Logging**: Comprehensive logging for debugging

### **Common Issues & Solutions**

#### **API Key Issues:**
```
❌ MAELYN_API_KEY not found in .env file
```
**Solution:** Create `.env` file and add `MAELYN_API_KEY=your_key_here`

#### **API Connection Failed:**
```
❌ API connection test failed
```
**Solutions:**
1. Check if API key is correct
2. Verify internet connection
3. Test via Admin Panel → Settings → Payment Config → Test API Connection

#### **Payment Creation Failed:**
```
❌ Failed to create payment
```
**Solutions:**
1. Verify Saweria username exists
2. Check API key permissions
3. Review debug info in admin panel

## 🛠️ **Maintenance**

### **System Monitoring**
- Real-time system status
- Payment statistics
- User analytics
- Database health checks

### **Backup Recommendations**
- Database: Regular SQLite backups
- Config: Version control for settings
- Logs: Rotate and archive logs

## 📝 **Changelog**

### **v2.1.0 - Enhanced Features & File Support**
- ✅ **File Products**: Added support for file upload and delivery
- ✅ **Enhanced Broadcast**: Image + Text broadcast functionality  
- ✅ **Product Details**: Advanced product description management
- ✅ **Admin CRUD**: Complete product editing interface
- ✅ **Payment Integration**: Corrected Maelyn API endpoints and authentication
- ✅ **Auto Delivery**: Smart delivery for both text and file products
- ✅ **Environment Config**: Added SAWERIA_USERNAME configuration

### **v2.0.0 - Complete Refactor**
- ✅ Replaced all payment methods with QRIS Saweria
- ✅ Implemented Maelyn API integration
- ✅ Added professional UI components
- ✅ Simplified admin interface with reply keyboards
- ✅ Introduced step-by-step wizards
- ✅ Added automatic payment monitoring (5-min intervals)
- ✅ Implemented auto-delivery system
- ✅ Added professional pagination and loading animations
- ✅ Removed old command-based interface
- ✅ Enhanced error handling and user feedback

## 🤝 **Support**

For support and customization:
- **Telegram**: Contact store owner via bot
- **Issues**: Report bugs through proper channels
- **Documentation**: This README covers all major features

## 📄 **License**

This project is for educational and commercial use. Please respect the terms of service of all integrated APIs.

---

**🔥 Powered by Modern Technology Stack**
- **Node.js** - Runtime
- **Telegraf** - Telegram Bot Framework
- **SQLite** - Database
- **Maelyn API** - Payment Processing
- **QRCode** - QR Generation

*Happy selling! 🛍️*
