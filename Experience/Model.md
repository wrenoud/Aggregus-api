    var ExperienceSchema = new mongoose.Schema({
        _id: String,
        _details: {
            date_created: String,
            approved: Boolean
        },
        name: String,
        description: String,
        guest_instructions: String,
        experience_details: {
            attendees: {
                max_total: Number,
                min_total: Number,
                max_booking: Number,
                min_booking: Number
            },
            dates: {
                abs_yes: Array,
                abs_no: Array
            },
            times: Array,
            restrict_dates: Boolean,
            permit_mixed: Boolean,
            duration: Number
        },
        photos: {
            cover: String,
            profile: String,
            gallery: String
        },
        creator: {
            type: String, 
            ref: 'User'
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
        price: Number
    });