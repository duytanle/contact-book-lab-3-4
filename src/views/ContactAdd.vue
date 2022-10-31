<template>
    <div class="page-add d-flex flex-column align-items-center col-12">
        <h4 class="mt-4">Thêm mới liên hệ</h4>
        <ContactForm :contact="contact" @submit:contact="addContact" class="col-4 mt-4" />
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
    data() {
        return {
            message: "",
            contact: {},
        };
    },
    methods: {
        async addContact(data) {
            try {
                await ContactService.create(data);
                this.message = "Tạo mới liên hệ thành công.";
            } catch (error) {
                console.log(error);
            }
        },
    },
    created() {
        this.message = "";
    },
};
</script>
<style scoped>

</style>
