<template>
    <section>
        <h3>Añadir profesor</h3>
        <div><label>Nombre:</label> <input type="text" v-model="teacher.teacherName" /></div>
        <div><label>Apellidos:</label> <input type="text" v-model="teacher.surname"/></div>
        <div><label>DNI / NIF:</label> <input type="text" v-model="teacher.dni"/></div>
        <div><label>Materias:</label> <input type="text" v-model="subject" /><button @click="handleSubject()">Agregar</button></div>
        <!-- Listado de agregados de materias: -->
        <div>
            <ul>
                <li v-for = "(elm, index) in teacher.subjects" v-bind:key="index">{{ elm }}</li>
            </ul>
        </div>
        
        <input type="checkbox" v-model="teacher.doc"/>Documentación Entregada
        <button @click="handleAddTeacher()">Agregar</button>       
    </section>
    
    <!-- Listado de profesores que tenemos agregados -->
    <section>
        <h3>Listado de profesores</h3>
        <table>
            <tr>
                <th>Nombre</th>
                <th>Apellidos</th>
                <th>DNI/NIF</th>
                <th>Materias</th>
                <th>Documentación Entregada</th>
            </tr>

            <tr v-for="elm in teachers" :key="elm.dni">
                <th>{{ elm.teacherName }}</th>
                <th>{{ elm.surname }}</th>
                <th>{{ elm.dni }}</th>  
                <th>
                    <ul>
                        <li v-for="(item, index) in elm.subjects" :key="index">{{ item }}</li>
                    </ul>
                </th>  
                <th v-if="elm.doc">Entregada</th>
                <th v-else>No Entregada</th>           
            </tr>
        </table>
    </section>
</template>


<script lang="ts" setup>
    import { Ref, ref } from 'vue'
    //Gestión de tipado de TypeScript:
    interface ITeacher{
        teacherName: string,
        surname: string,
        dni: string,
        subjects: Array<string>,
        doc: boolean,
    }
    //Información que vamos a estar guardando del profesor:
    let teacher:Ref<ITeacher> = ref({
        teacherName: '',
        surname: '',
        dni: '',
        subjects: [],
        doc: false,
    })
    //Array para almacenar varios profesores:
    let teachers:Ref<Array<ITeacher>> = ref([])
    //Guardar materias agregadas:
    let subject:Ref<string> = ref('')
    //Agregar materias:
    const handleSubject = () => {
        teacher.value.subjects.push(subject.value)
        // Dejar el casillero en blanco luego de agregar materia:
        subject.value = ""
    }
    //Agregar profesor completo a listado de profesores:
    const handleAddTeacher = () => {
        //En la siguiente linea pasaremos un objeto no referenciado, no reactivo para que los valores queden guardados:
        teachers.value.push({
            teacherName: teacher.value.teacherName,
            surname: teacher.value.surname,
            dni: teacher.value.dni,
            subjects: teacher.value.subjects,
            doc: teacher.value.doc
        })
        teacher.value.teacherName = ""
        teacher.value.surname = ""
        teacher.value.dni = ""
        teacher.value.subjects = []
        teacher.value.doc = false
    }
</script>


<style scoped>

</style>
