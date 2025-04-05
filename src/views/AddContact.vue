<template>
    <div class="page">
        <h2 class="page-heading">Add New Contact</h2>
        <div class="form-fields">
            <div class="name-row">
                <ui-textfield v-model="contact.firstName" label="First Name" outlined required></ui-textfield>
                <ui-textfield v-model="contact.lastName" label="Last Name" outlined required></ui-textfield>
            </div>
            <div class="phone-age-row">
                <ui-textfield v-model="contact.phone" label="Phone (10 digits)" outlined required></ui-textfield>
                <ui-textfield v-model.number="contact.age" type="number" label="Age" outlined required></ui-textfield>
            </div>
            <ui-textfield v-model="contact.email" label="Email" outlined required></ui-textfield>
            <ui-form-field>
                <ui-radio v-model="contact.gender" input-id="male" value="male"></ui-radio>
                <label for="male">Male</label>
                <ui-radio v-model="contact.gender" input-id="female" value="female"></ui-radio>
                <label for="female">Female</label>
                <ui-radio v-model="contact.gender" input-id="other" value="other"></ui-radio>
                <label for="other">Other</label>
            </ui-form-field>
            <ui-button class="mt" color="primary" @click="saveContact" raised>Save Contact</ui-button>
        </div>
    </div>
</template>

<script setup>
import { reactive } from 'vue';
import { useRouter } from 'vue-router';

const router = useRouter();

const contact = reactive({
    id: Date.now(),
    firstName: '',
    lastName: '',
    phone: '',
    email: '',
    gender: 'male',
    age: null
});

const saveContact = () => {
    if (!contact.firstName || !contact.lastName || !contact.email || !contact.phone) {
        alert('Please fill in all required fields.');
        return;
    }

    const contacts = JSON.parse(localStorage.getItem('contacts') || '[]');
    contacts.push(contact);
    localStorage.setItem('contacts', JSON.stringify(contacts));

    router.push({ name: 'ContactDetails', params: { id: contact.id } });
};
</script>

<style scoped>
.page-heading {
    font-size: 24px;
    margin-bottom: 20px;
    text-align: center;
    color: #333;
}

.form-fields {
    display: flex;
    flex-direction: column;
    gap: 16px;
    max-width: 600px;
    margin: 0 auto;
}

.name-row,
.phone-age-row {
    display: flex;
    gap: 16px;
    justify-content: space-between;
}

.mt {
    margin-top: 20px;
}

.mdc-text-field {
    width: 100%;
}
</style>
