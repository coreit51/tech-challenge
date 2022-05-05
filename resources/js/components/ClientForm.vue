<template>
    <div>
        <h1 class="mb-6">Clients -> Add New Client</h1>

        <div class="max-w-lg mx-auto">
            <p v-if="errors.length" class="error-sec">
    <b>Please correct the following error(s):</b>
    <ul>
      <li v-for="error in errors">{{ error }}</li>
    </ul>
  </p>

            <div class="form-group">
                <label for="name">Name</label>
                <input
                    type="text"
                    id="name"
                    class="form-control"
                    v-model="client.name"
                />
            </div>
            <div class="form-group">
                <label for="email">Email</label>
                <input
                    type="text"
                    id="email"
                    class="form-control"
                    v-model="client.email"
                />
            </div>
            <div class="form-group">
                <label for="phone">Phone</label>
                <input
                    type="text"
                    id="phone"
                    class="form-control"
                    v-model="client.phone"
                />
            </div>
            <div class="form-group">
                <label for="name">Address</label>
                <input
                    type="text"
                    id="address"
                    class="form-control"
                    v-model="client.address"
                />
            </div>
            <div class="flex">
                <div class="form-group flex-1">
                    <label for="city">City</label>
                    <input
                        type="text"
                        id="city"
                        class="form-control"
                        v-model="client.city"
                    />
                </div>
                <div class="form-group flex-1">
                    <label for="postcode">Postcode</label>
                    <input
                        type="text"
                        id="postcode"
                        class="form-control"
                        v-model="client.postcode"
                    />
                </div>
            </div>

            <div class="text-right">
                <a href="/clients" class="btn btn-default">Cancel</a>
                <button @click="storeClient" class="btn btn-primary">
                    Create
                </button>
            </div>
        </div>
    </div>
</template>

<script>
import axios from "axios";

export default {
    name: "ClientForm",

    data() {
        return {
            client: {
                name: "",
                email: "",
                phone: "",
                address: "",
                city: "",
                postcode: ""
            },
            errors: []
        };
    },

    methods: {
        storeClient(e) {
            this.errors = [];
            if (!this.client.name) {
                this.errors.push("Name is required.");
            } else if (this.client.name.length > 190) {
                this.errors.push(
                    "Name should be be greater than 190 characters."
                );
            }

            if(this.client.email && !/^\w+([\.-]?\w+)*@\w+([\.-]?\w+)*(\.\w{2,3})+$/.test(this.client.email)) {
                this.errors.push("Please enter a valid email address.");
            }

            if(this.client.phone && !/^[\+\d]?(?:[\d\s]*)$/.test(this.client.phone)) {
                this.errors.push("Please enter a valid phone.");
            } else if(this.client.phone && (this.client.phone.length < 5 || this.client.phone.length > 15)) {
                this.errors.push("Length of the phone number should be min of 5 digits and upto 15 digits.");
            }

            if(!this.client.phone && !this.client.email) {
                this.errors.push("At least one of (phone/email) is required");
            }

            if(!this.errors.length) {
                axios.post("/clients", this.client).then(data => {
                    window.location.href = data.data.url;
                });
            }

            e.preventDefault();
        }
    }
};
</script>

<style scoped>
.error-sec li {
    color: red;
}
</style>