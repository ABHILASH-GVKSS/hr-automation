# Credit System Documentation

## Overview
The application now includes a credit-based system for scheduling calls. Users must have sufficient credits to schedule calls, and credits are deducted when calls are successfully scheduled.

## Credit Rules

### Credit Deduction
- **Cost per call**: 1 credit
- **When deducted**: When a call is successfully scheduled
- **Minimum requirement**: 1 credit to schedule a call

### Credit Refunds
- **When refunded**: When a call is cancelled or deleted before it's made
- **Refund amount**: 1 credit
- **Conditions for refund**:
  - Call status is 'Scheduled' or 'pending'
  - Call has not been made yet (not 'in-progress', 'completed', or 'failed')

### Credit Management
- **Admin control**: Admins can add credits to user accounts
- **User visibility**: Users can see their current credit balance
- **Real-time updates**: Credit display updates immediately after operations

## Implementation Details

### Backend Changes

#### 1. Schedule Call Endpoint (`/api/schedule-call`)
- Checks if user has sufficient credits (minimum 1)
- Deducts 1 credit when call is scheduled successfully
- Returns updated credit balance in response

#### 2. Call Cancellation Endpoint (`/api/scheduler/cancel/:callId`)
- Refunds 1 credit if call is cancelled before being made
- Only refunds for calls with status 'Scheduled' or 'pending'

#### 3. Call Deletion Endpoint (`/api/user-calls/:callId`)
- Refunds 1 credit if call is deleted before being made
- Only refunds for calls with status 'Scheduled' or 'pending'

### Frontend Changes

#### 1. User Dashboard
- Shows current credit balance in navigation bar
- Displays credit information in scheduling form
- Prevents scheduling if insufficient credits
- Updates credit display after operations

#### 2. Scheduling Form
- Shows current credits and cost per call
- Disables submit button if insufficient credits
- Displays helpful messages about credit requirements

#### 3. Call Management
- Cancel and delete buttons for scheduled calls
- Credit refund notifications
- Real-time credit balance updates

## User Experience

### Scheduling a Call
1. User clicks "Scheduling Call" card
2. Form shows current credits and cost (1 credit)
3. If insufficient credits, form is disabled with clear message
4. If sufficient credits, user can fill form and submit
5. 1 credit is deducted and remaining balance is shown

### Cancelling a Call
1. User clicks "Cancel" button on scheduled call
2. Confirmation dialog appears
3. If confirmed, call is cancelled
4. If call was pending, 1 credit is refunded
5. User sees notification with refund information

### Deleting a Call
1. User clicks "Delete" button on scheduled call
2. Confirmation dialog appears
3. If confirmed, call is deleted
4. If call was pending, 1 credit is refunded
5. User sees notification with refund information

## Admin Features

### Managing User Credits
- Admins can view all users' credit balances
- Admins can edit user credits through admin dashboard
- Credit changes are reflected immediately

### Monitoring
- Server logs show credit deductions and refunds
- Admin dashboard displays user credit information
- Credit transactions are logged with user details

## Error Handling

### Insufficient Credits
- Frontend prevents form submission
- Backend returns 400 error with clear message
- User sees helpful notification

### Network Errors
- Graceful handling of network failures
- User-friendly error messages
- Credit operations are atomic (all-or-nothing)

## Testing

### Test Scenarios
1. **Normal scheduling**: User with sufficient credits schedules call
2. **Insufficient credits**: User with 0 credits tries to schedule call
3. **Credit refund**: User cancels/deletes pending call
4. **No refund**: User cancels/deletes completed call
5. **Admin operations**: Admin modifies user credits

### Test Commands
```bash
# Run credit system test
node test-credit-deduction.js

# Check server logs for credit operations
# Look for messages starting with 💳
```

## Security Considerations

### Credit Validation
- Server-side validation of credit availability
- Prevents negative credit balances
- Ensures credit operations are atomic

### User Authorization
- Users can only manage their own credits
- Admins can manage all users' credits
- Proper authentication required for all operations

## Future Enhancements

### Potential Features
- Credit purchase system
- Credit expiration dates
- Bulk credit operations
- Credit usage analytics
- Credit tier system (different costs for different call types)

### Monitoring Improvements
- Credit transaction history
- Usage analytics dashboard
- Automated credit alerts
- Credit balance reports 