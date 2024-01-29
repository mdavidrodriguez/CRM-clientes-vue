<script setup>
import { FormKit } from '@formkit/vue'
import RouterLink from '../components/UI/RouterLink.vue';
import Heading from '../components/UI/Heading.vue'
import { useRouter } from 'vue-router'
import ClienteService from '../services/ClienteService';
const router = useRouter()

defineProps({
    titulo: {
        type: String
    }
})
const handleSubmit = (data) => {
    data.estado = 1
    ClienteService.agregarCliente(data)
        .then(respuesta => {
            console.log(respuesta);
            // redireccionar
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
                <FormKit type="form" submit-label="Agregar Cliente"
                    incomplete-message="No se pudo enviar, revisa los mensajes" @submit="handleSubmit">
                    <FormKit type="text" label="Nombre" placeholder="Nombre del Cliente" validation="required"
                        :validation-messages="{ required: 'El Nombre del cliente es obligatorio' }" name="nombre" />
                    <FormKit type="text" label="Apellido" placeholder="Apellido del Cliente" validation="required"
                        :validation-messages="{ required: 'El Apellido del cliente es obligatorio' }" name="apellido" />
                    <FormKit type="email" label="Email" placeholder="Email del Cliente" validation="required|email"
                        :validation-messages="{ required: 'El Email del cliente es obligatorio', email: 'Coloca un email válido' }"
                        name="email" />
                    <FormKit type="text" label="Telefono" placeholder="Telefono XXX-XXX-XXXX"
                        validation="?matches:/^[0-9]{3}-[0-9]{3}-[0-9]{4}$/"
                        :validation-messages="{ matches: 'El Formato no es valido' }" name="telefono" />
                    <FormKit type="text" label="Empresa" placeholder="Empresa del Cliente" name="empresa" />
                    <FormKit type="text" label="Puesto" placeholder="Puesto del Cliente" name="puesto" />
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