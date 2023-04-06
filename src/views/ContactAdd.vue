<template>
    <div v-if="contact" class="page">
        <h4>Thêm Liên hệ</h4>
        <ContactForm :contact="contact" @submit:contact="addContact" @delete:contact="deleteContact" />
        <p>{{ message }}</p>
    </div>
</template>
<script>
import ContactForm from "@/components/ContactForm.vue";

import ContactService from "@/services/contact.service";
export default {
    components: {
        ContactForm,
    },
    data() {
        return {
            contact: {},
            message: "",
        };
    },
    methods: {
        async addContact(data) {
            try {
                await ContactService.create(data);
                this.message = "Liên hệ đã được thêm thành công. Hệ thống sẽ tự động trở về trang chủ sau 3s";
                setTimeout(() => this.$router.push({ name: "contactbook" }), 3000);
            } catch (error) {
                console.log(error);
            }
        },
        async deleteContact() {
            if (confirm("Bạn muốn xóa Liên hệ này?")) {
                try {
                    await ContactService.delete(this.contact._id);
                    this.$router.push({ name: "contactbook" });
                } catch (error) {
                    console.log(error);
                }
            }
        },
    },
};
</script>