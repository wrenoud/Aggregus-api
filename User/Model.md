```JSON
UserSchema = new mongoose.Schema({
    _id: String,
    _details: {
        date_created: Date,
        type: String,
        terms_agree: Boolean,
        email_confirm: Boolean,
        deleted: Boolean,
        password_reset: Boolean
    },
    name: {
        first: String,
        middle: String,
        last: String,
        business_name: String
    },
    password: String,
    description: String,
    photos: {
        cover: String,
        profile: String,
        gallery: String
    },
    location: {
        normal: {
            formatted_address: String,
            city: String,
            postal_code: Number,
            county: String,
            country: String  
        },
        lat: Number,
        lng: Number
    },
    contact: {
        email: String,
        phone: Number,
        address: {
            street: {
                one: String,
                two: String
            },
            city: String,
            state: String,
            zip: Number
        }
    },
    social: {
        facebook: {
            id: Number,
            username: String
        },
        twitter: {
            id: Number,
            username: String
        },
        pinterest: {
            id: Number,
            username: String
        }
    },
    financial: {
        customer_account: Object, // Stripe customer object
        bank_account: Object, // Stripe recipient object
        balance_owed: Object
    }
});
```