<template>
    <div>
        <h1 class="mb-6">Clients -> Add New Client</h1>

        <div class="max-w-lg mx-auto">
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
            }
        };
    },

    methods: {
        storeClient(e) {
            this.errors = [];

            let check = false;
            if (!this.client.name) {
                this.errors.push("Name required.");
            } else if (this.client.name.length > 190) {
                this.errors.push(
                    "Name should be be greater than 190 characters."
                );
            } else if (!this.client.email || this.client.email) {
                check = true;
            } else if (
                /^\w+([\.-]?\w+)*@\w+([\.-]?\w+)*(\.\w{2,3})+$/.test(
                    this.client.email
                )
            ) {
                console.log("a");

                this.errors.push("Please enter a valid email address.");
            } else if (/^[0-9]$/.test(this.client.phone)) {
                console.log("b");

                this.errors.push("Please enter a valid phone number.");
            } else {
                if (check == true) {
                    console.log("comes");
                    axios.post("/clients", this.client).then(data => {
                        window.location.href = data.data.url;
                    });
                } else {
                    this.errors.push("Please fill email or phone number.");
                }
            }
            e.preventDefault();
        }
    }
};
</script>
