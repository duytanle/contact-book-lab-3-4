<template>
    <div class="page col-12 flex-grow-1">
        <div class="contact__search col-md-6">
            <InputSearch v-model="searchText" class="mt-2"/>
        </div>
        <div class="contact__info mt-3 d-flex justify-content-between ">
            <div class="contact__list col-md-6 mt-3 p-4 rounded-2">
                <h4 class="mb-4 border-dark border-bottom pb-3">
                    Danh bạ
                    <i class="fas fa-address-book"></i>
                </h4>
                <ContactList v-if="filteredContactsCount > 0" :contacts="filteredContacts" v-model:activeIndex="activeIndex" />
                <p v-else>Không có liên hệ nào.</p>
            </div>
            <div class="contact__detail  col-md-6 rounded-3 mt-3 p-4" v-if="activeContact">
                <div >
                    <h4 class="mb-3 border-dark border-bottom pb-3">
                        Chi tiết Liên hệ
                        <i class="fas fa-address-card"></i>
                    </h4>
                    <ContactCard :contact="activeContact" />
                    <router-link :to="
                        {
                        name: 'contact.edit',
                        params: { id: activeContact._id },
                        }"
                    >
                        <span class="mt-2">
                            <i class="fas fa-edit"></i> Hiệu chỉnh
                        </span>
                    </router-link>
                </div>
            </div>
        </div>
        <div class="contact__button mt-4 text-center">
            <button class="btn btn-primary me-3" @click="refreshList()">
                <i class="bi bi-arrow-clockwise"></i> Làm mới
            </button>
            <button class="btn btn-success me-3" @click="goToAddContact">
                <i class="bi bi-plus-circle"></i> Thêm mới
            </button>
            <button class="btn btn-danger" @click="removeAllContacts">
                <i class="bi bi-trash"></i> Xóa tất cả
            </button>
        </div>
    </div>

</template>

<script>
import ContactCard from '../components/ContactCard.vue';
import ContactList from '../components/ContactList.vue';
import InputSearch from '../components/InputSearch.vue';
import ContactService from '../services/contact.service.js'
export default {
    components: { InputSearch, ContactList, ContactCard },
    data() {
        return {
            contacts: [],
            activeIndex: -1,
            searchText: "",
        };
    },
    watch: {
        // Giám sát các thay đổi của biến searchText.
        // Bỏ chọn phần tử đang được chọn trong danh sách.
        searchText() {
            this.activeIndex = -1;
        },
    },
    computed: {
        // Chuyển các đối tượng contact thành chuỗi để tiện cho tìm kiếm.
        contactStrings() {
            return this.contacts.map((contact) => {
                const { name, email, address, phone } = contact;
                return [name, email, address, phone].join("");
            });
        },
        // Trả về các contact có chứa thông tin cần tìm kiếm.
        filteredContacts() {
            if (!this.searchText) return this.contacts;
            return this.contacts.filter((_contact, index) =>
                this.contactStrings[index].includes(this.searchText)
            );
        },
        activeContact() {
            if (this.activeIndex < 0) return null;
            return this.filteredContacts[this.activeIndex]; 
        },
        filteredContactsCount() {
            return this.filteredContacts.length;
        },
    },
    methods: {
        async retrieveContacts() {
            try {
                this.contacts = await ContactService.getAll();
            } catch (error) {
                console.log(error);
            }
        },
        refreshList() {
            this.retrieveContacts();
            this.activeIndex = -1;
        },
        async removeAllContacts() {
            if (confirm("Bạn muốn xóa tất cả Liên hệ?")) {
                try {
                    await ContactService.deleteAll();
                    this.refreshList();
                } catch (error) {
                    console.log(error);
                }
            }
        },
        goToAddContact() {
            this.$router.push({ name: "contact.add" });
        },
    },
    mounted() {
        this.refreshList();
    },
};
</script>

<style scoped>
.page {
    text-align: left;
}

.contact__list{
    max-width: 450px;
    box-shadow: rgba(99, 99, 99, 0.2) 0px 2px 8px 0px;
    
}
.contact__detail{
    max-height: 30 0px;
    box-shadow: rgba(99, 99, 99, 0.2) 0px 2px 8px 0px;
}

.contact__button{
    width: 450px;
}
</style>
