<script setup>
import { FormKit, } from '@formkit/vue'
import { onMounted, reactive } from 'vue'
import RouterLink from '../components/UI/RouterLink.vue';
import Heading from '../components/UI/Heading.vue'
import { useRoute, useRouter } from 'vue-router'
import ClienteService from '../services/ClienteService';

const router = useRouter()
const route = useRoute()


const { id } = route.params

const formData = reactive({
    nombre: '',
    apellido: '',
    email: '',
    telefono: '',
    empresa: '',
    puesto: ''
})

onMounted(() => {
    ClienteService.obtenerCliente(id)
        .then(({ data }) => {
            // Forma larga
            // -------------------------
            // formData.nombre = data.nombre
            // formData.apellido = data.apellido
            // formData.email = data.email
            // formData.telefono = data.telefono
            // formData.empresa = data.empresa
            // formData.puesto = data.puesto
            Object.assign(formData, data)
        })
        .catch(error => console.log(error))
})

defineProps({
    titulo: {
        type: String
    }
})
const handleSubmit = (data) => {
    ClienteService.editarCliente(id, data)
        .then(respuesta => {
            router.push({ name: "listado-clientes" })
        })
        .catch(error => console.log(error))
}
</script>
<template>
    <div>
        <div class="flex justify-end">
            <RouterLink to="listado-clientes">
                Volver
            </RouterLink>
        </div>
        <Heading>{{ titulo }}</Heading>
        <div class="mx-auto mt-10 bg-white shadow">
            <div class="mx-auto md:w-2/3 py-20 px-6">
                <FormKit type="form" submit-label="Guardar Cambios"
                    incomplete-message="No se pudo enviar, revisa los mensajes" @submit="handleSubmit" :value="formData">
                    <FormKit type="text" label="Nombre" placeholder="Nombre del Cliente" validation="required"
                        :validation-messages="{ required: 'El Nombre del cliente es obligatorio' }" name="nombre"
                        v-model="formData.nombre" />
                    <FormKit type="text" label="Apellido" placeholder="Apellido del Cliente" validation="required"
                        :validation-messages="{ required: 'El Apellido del cliente es obligatorio' }" name="apellido"
                        v-model="formData.apellido" />
                    <FormKit type="email" label="Email" placeholder="Email del Cliente" validation="required|email"
                        :validation-messages="{ required: 'El Email del cliente es obligatorio', email: 'Coloca un email válido' }"
                        name="email" v-model="formData.email" />
                    <FormKit type="text" label="Telefono" placeholder="Telefono XXX-XXX-XXXX"
                        validation="?matches:/^[0-9]{3}-[0-9]{3}-[0-9]{4}$/"
                        :validation-messages="{ matches: 'El Formato no es valido' }" name="telefono"
                        v-model="formData.telefono" />
                    <FormKit type="text" label="Empresa" placeholder="Empresa del Cliente" name="empresa"
                        v-model="formData.empresa" />
                    <FormKit type="text" label="Puesto" placeholder="Puesto del Cliente" name="puesto"
                        v-model="formData.puesto" />
                </FormKit>
            </div>
        </div>
    </div>
</template>

<style>
.formkit-wrapper {
    max-width: 100%;
}
</style>