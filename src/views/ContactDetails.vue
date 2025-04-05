<template>
    <div class="page">
        <h2 class="page-heading">Contact Details</h2>
        <div v-if="contact" class="contact-details">
            <div class="details">
                <p><strong>Name:</strong> {{ contact.firstName }} {{ contact.lastName }}</p>
                <p><strong>Phone:</strong> {{ contact.phone }}</p>
                <p><strong>Email:</strong> {{ contact.email }}</p>
                <p><strong>Gender:</strong> {{ contact.gender }}</p>
                <p><strong>Age:</strong> {{ contact.age }}</p>
            </div>
            <div class="action-buttons">
                <ui-button color="primary" class="mr" @click="editContact" raised icon="edit">
                    Edit
                </ui-button>
                <ui-button color="error" class="mr" @click="deleteContact" raised icon="delete">
                    Delete
                </ui-button>
                <ui-button color="secondary" @click="goToHome" raised icon="home">
                    Home
                </ui-button>
            </div>
        </div>
        <div v-else>
            <p>Contact not found.</p>
        </div>
    </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import { useRoute, useRouter } from 'vue-router';

const route = useRoute();
const router = useRouter();

const contact = ref(null);

const loadContact = () => {
    const contacts = JSON.parse(localStorage.getItem('contacts') || '[]');
    contact.value = contacts.find(con => con.id === Number(route.params.id));
};

onMounted(loadContact);

const editContact = () => {
    router.push({ name: 'EditContact', params: { id: contact.value.id } });
};

const deleteContact = () => {
    const confirmed = confirm('Are you sure you want to delete this contact?');
    if (confirmed) {
        const contacts = JSON.parse(localStorage.getItem('contacts') || '[]');
        const updated = contacts.filter(con => con.id !== contact.value.id);
        localStorage.setItem('contacts', JSON.stringify(updated));
        router.push({ name: 'ContactList' });
    }
};

const goToHome = () => {
    router.push({ name: 'ContactList' });
};
</script>

<style scoped>
.page {
    max-width: 800px;
    margin: 0 auto;
    text-align: center;
}

.page-heading {
    font-size: 32px;
    margin-bottom: 20px;
    color: #333;
}

.contact-details {
    background: #f9f9f9;
    padding: 20px;
    border-radius: 10px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    margin-bottom: 20px;
}

.details p {
    font-size: 16px;
    margin-bottom: 12px;
    color: #555;
}

.action-buttons {
    display: flex;
    justify-content: center;
    gap: 20px;
    margin-top: 20px;
}

.mr {
    margin-right: 10px;
}

.ui-button {
    display: flex;
    align-items: center;
    gap: 8px;
    padding: 12px 24px;
    font-weight: bold;
}

.ui-icon {
    font-size: 15px;
}
</style>
