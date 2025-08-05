# Razorpay Integration Setup Guide

## Overview
This Laravel project now includes Razorpay payment integration alongside the existing Cash on Delivery (COD) option.

## Features Added
1. **Razorpay Payment Button**: Added alongside COD button on cart page
2. **Payment Processing**: Secure payment processing with signature verification
3. **Order Storage**: All payment details stored in the same bookings table
4. **User Association**: Orders are associated with logged-in users
5. **Cart Management**: Cart is automatically cleared after successful payment

## Setup Instructions

### 1. Environment Configuration
Add the following variables to your `.env` file:

```env
RAZORPAY_KEY=your_razorpay_key_id
RAZORPAY_SECRET=your_razorpay_secret_key
```

### 2. Get Razorpay Credentials
1. Sign up for a Razorpay account at https://razorpay.com
2. Go to Settings > API Keys
3. Generate a new API key pair
4. Copy the Key ID and Key Secret to your .env file

### 3. Database Migration
The following migration has been added to support Razorpay:
- `add_razorpay_fields_to_bookings_table.php`

Run migrations if not already done:
```bash
php artisan migrate
```

### 4. New Routes Added
- `POST /create-razorpay-order` - Creates Razorpay order
- `POST /process-razorpay-payment` - Processes payment after completion

### 5. Controller Methods Added
In `CartController.php`:
- `createRazorpayOrder()` - Creates Razorpay order
- `processRazorpayPayment()` - Processes and verifies payment

### 6. Database Fields Added
The `bookings` table now includes:
- `razorpay_order_id` - Razorpay order ID
- `razorpay_payment_id` - Razorpay payment ID  
- `razorpay_signature` - Payment signature for verification
- `user_id` - Associated user ID

## Usage

### For Users:
1. Add items to cart
2. Go to cart page
3. Choose between "Cash on Delivery" or "Pay with Razorpay"
4. If Razorpay is selected, the payment modal will open
5. Complete payment using any supported method
6. Order is automatically created and cart is cleared

### For Developers:
- All business logic is contained within `CartController`
- Payment verification is handled server-side
- Error handling and validation included
- Session-based user association maintained

## Security Features
- Payment signature verification
- Server-side order creation
- CSRF protection
- Input validation
- Error handling

## Testing
For testing, use Razorpay's test mode:
- Test cards: 4111 1111 1111 1111
- Test UPI: success@razorpay
- Test wallets: Paytm, PhonePe, etc.

## Error Handling
- Empty cart validation
- Payment verification failures
- Network errors
- Invalid payment data

## Notes
- Amount is converted to paise (Razorpay requirement)
- Orders are marked as 'razorpay_paid' for payment status
- All existing COD functionality remains unchanged
- Cart is cleared only after successful payment verification 