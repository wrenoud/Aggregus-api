    var BookingSchema = new mongoose.Schema({
        _id: String,
        _details: {
            date_created: String,
            confirmed: Boolean,
            charged: Boolean,
            charge_token: String
        },
        booking_details: {
            date_booked: {
                date: String,
                times: String
            },
            attendees: String,
            total_price: Number,
            host_profit: Number
        },
        creator: {
            type: String,
            ref: 'User'
        },
        recipient: {
            type: String,
            ref: 'User'
        },
        experience: {
            type: String,
            ref: 'User'
        }
    });