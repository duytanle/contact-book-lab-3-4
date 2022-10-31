<template>
    <div v-if="contact" class="page-edit d-flex flex-column align-items-center col-12">
        <h4 class="mt-4">Hiệu chỉnh Liên hệ</h4>
        <ContactForm :contact="contact" @submit:contact="updateContact" @delete:contact="deleteContact" class="col-4 mt-4"/>
        <p class="mt-3 fw-bold" style="color:blue">{{ message }}</p>
    </div>
</template>
<script>
import ContactForm from '../components/ContactForm.vue';
import ContactService from '../services/contact.service';
export default {
    components: {
        ContactForm,
    },
    props: {
        id: { type: String, required: true },
    },
    data() {
        return {
            contact: null,
            message: "",
        };
    },
    methods: {
        async getContact(id) {
            try {
                this.contact = await ContactService.get(id);
            } catch (error) {
                console.log(error);
                // Chuyển sang trang NotFound đồng thời giữ cho URL không đổi
                this.$router.push({
                    name: "notfound",
                    params: {
                        pathMatch: this.$route.path.split("/").slice(1)
                    },
                    query: this.$route.query,
                    hash: this.$route.hash,
                });
            }
        },

        async updateContact(data) {
            try {
                await ContactService.update(this.contact._id, data);
                this.message = "Liên hệ được cập nhật thành công.";
            } catch (error) {
                console.log(error);
            }
        },
        async deleteContact() {
            if (confirm("Bạn muốn xóa Liên hệ này?")) {
                try {
                    await ContactService.delete(this.contact._id);
                    this.message = "Liên hệ được xóa thành công.";
                    this.$router.push({ name: "contactbook" });
                } catch (error) {
                    console.log(error);
                }
            }
        },
    },
    created() {
        this.getContact(this.id);
        this.message = "";
    },
};
</script>
<style scoped>

</style>