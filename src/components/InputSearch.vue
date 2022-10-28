<template>
<div class="search input-group">
    <input type="text" class="form-control rounded-3" placeholder="Nhập thông tin cần tìm" :value="modelValue"
        @input="updateModelValue" @keyup.enter="submit" />
    
    <div class="input-group-append ms-3">
        <button class="btn btn-primary" type="button" @click="submit">
            <i class="bi bi-search fs-5 me-2"></i> 
            <span>Tìm kiếm</span>    
        </button>
    </div>
</div>
</template>

<script>
export default {
    props: {
        modelValue: { type: String, default: "" },
    },
    emits: ["submit", "update:modelValue"],
    methods: {
        updateModelValue(e) {
            this.$emit("update:modelValue", e.target.value);
        },
        
        submit() {
            this.$emit("submit");
        },

    },
}
</script>

<style scoped>
</style>