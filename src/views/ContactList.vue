<template>
    <div class="page">
        <div class="search-add-container">
            <ui-textfield v-model="searchQuery" label="Search contacts" outlined class="search-input"></ui-textfield>
        </div>
        <ui-list :type="2" avatar v-if="filteredContacts.length > 0">
            <template v-for="contact in filteredContacts" :key="contact.id">
                <ui-item @click="goToDetails(contact.id)">
                    <template #before="{ iconClass }">
                        <ui-icon :class="iconClass">person</ui-icon>
                    </template>
                    <ui-item-text-content>
                        <ui-item-text1>{{ contact.lastName }}, {{ contact.firstName }}</ui-item-text1>
                        <ui-item-text2>{{ contact.email }}</ui-item-text2>
                    </ui-item-text-content>
                    <ui-item-last-content>
                        <ui-icon @click="goToEditContact(contact.id)">edit</ui-icon>
                    </ui-item-last-content>
                </ui-item>
            </template>
        </ui-list>
        <div class="no-contacts" v-else>
            <ui-icon class="icon">contact_page</ui-icon>
            <p>No contacts found.</p>
        </div>
    </div>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue';
import { useRouter } from 'vue-router';

const router = useRouter();
const contacts = ref([]);
const searchQuery = ref('');

const loadContacts = () => {
    const stored = JSON.parse(localStorage.getItem('contacts') || '[]');
    contacts.value = stored.sort((a, b) => a.lastName.localeCompare(b.lastName));
};

const goToDetails = (id) => {
    router.push({ name: 'ContactDetails', params: { id } });
};


const goToEditContact = (id) => {
    router.push({ name: 'EditContact', params: { id } });
};

const filteredContacts = computed(() => {
    return contacts.value.filter(contact => {
        const fullName = `${contact.firstName} ${contact.lastName}`.toLowerCase();
        return fullName.includes(searchQuery.value.toLowerCase());
    });
});

onMounted(loadContacts);
</script>

<style scoped>
.search-add-container {
    display: flex;
    gap: 16px;
    margin-bottom: 20px;
    align-items: center;
}

.search-input {
    flex-grow: 1;
}

.add-button {
    white-space: nowrap;
}

.ui-item {
    padding: 12px;
    cursor: pointer;
}

.ui-item-text1 {
    font-weight: bold;
}

.ui-item-text2 {
    font-size: 14px;
    color: #555;
}

.ui-icon {
    cursor: pointer;
}

.no-contacts {
    text-align: center;
    margin-top: 50px;
    color: #888;
}

.no-contacts .icon {
    font-size: 48px;
    color: #bbb;
    margin-bottom: 10px;
}
</style>
