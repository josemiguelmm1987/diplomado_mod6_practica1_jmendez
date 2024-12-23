<template>
    <div class="about">
        <h5 class="mb-5 mt-5">Mi Agenda</h5>
        
        <div>
            <!-- <div class="mb-3">
                <div class="input-group">
                    <span class="input-group-text" id="type-filter"><i
                            class="bi bi-filter "></i>  Filtrar por País </span>
                    <select class="form-select" @change="toFilter = $event.target.value">
                        <option value="">Todos</option>
                        <option v-for="country in countries" :value="country" :key="country">{{country}}</option>
                    </select>
                </div>
            </div> -->

            <div class="mb-3">
                <div class="input-group">
                    <span class="input-group-text" id="type-filter"><i
                            class="bi bi-filter "></i>  Filtrar por Nombre </span>
                    <select class="form-select" @change="toFilter = $event.target.value">
                        <option value="">Todos</option>
                          <option v-for="name in agendaArray.map(agenda => agenda.name)" :value="name" :key="name">{{name}}</option>
                    </select>
                </div>
            </div>

            <div class="mb-3">
                <div class="input-group">
                    <span class="input-group-text" id="name-search-text"><i class="bi bi-search"></i> Buscar por Nombre </span>
                    <input type="search" class="form-control" id="name-search"
                           @search="this.toSearch = $event.target.value">
                </div>
            </div>
        </div>
        
        <div class="card">
            <div class="card-body">
                <form @submit.prevent="save()">
                    <table class="table table-striped">
                        <thead>
                            <tr>
                                <th colspan="2">
                                    <input type="text" class="form-control" id="name" v-model="newAgenda.name">
                                </th>
                                <th>
                                    <input type="email" class="form-control" id="email" v-model="newAgenda.email">
                                </th>
                                <th>
                                    <input type="text" class="form-control" id="address" v-model="newAgenda.address">
                                </th>
                                <th>
                                    <input type="tel" class="form-control" id="phone" v-model="newAgenda.phone">
                                </th>
                                <th>
                                    <select class="form-select" aria-label="Default select example" v-model="newAgenda.country">
                                        <option v-for="country in countries" :value="country" :key="country">{{country}}</option>
                                    </select>
                                </th>
                                <th>
                                    <input type="text" class="form-control" id="city" v-model="newAgenda.city">
                                </th>
                                <th>
                                    <button type="submit" class="btn btn-primary">Agregar</button>
                                </th>
                            </tr>
                            <tr>
                                <th scope="col">#</th>
                                <th scope="col">Nombre</th>
                                <th scope="col">Correo Electrónico</th>
                                <th scope="col">Dirección</th>
                                <th scope="col">Teléfono</th>
                                <th scope="col">País</th>
                                <th scope="col">Ciudad</th>
                                <th scope="col">Acciones</th>
                            </tr>
                        </thead>

                        <tbody>
                            <tr v-for="(agenda, index) in getList()" :key="index">
                                <!-- <td>{{ agenda.id }}</td> -->
                                <th scope="row">{{1+index}}</th> 
                                <td>{{ agenda.name }}</td>
                                <td>{{ agenda.email }}</td>
                                <td>{{ agenda.address }}</td>
                                <td>{{ agenda.phone }}</td>
                                <td>{{ agenda.country }}</td>
                                <td>{{ agenda.city }}</td>
                                <td>
                                    <!-- <button @click="edit(index)" class="btn btn-primary">Editar</button>
                                    <button @click="remove(index)" class="btn btn-danger">Eliminar</button> -->
                                    <button type="button" class="btn btn-info btn-sm" @click="edit(index)"><i
                                        class="bi bi-pen"></i></button>
                                    <button type="button" class="btn btn-danger btn-sm" @click="remove(index)"><i
                                        class="bi bi-trash3"></i></button>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </form>
            </div>
        </div>

        <div class="modal fade" id="editModal" tabindex="-1" aria-labelledby="editModalLabel" aria-hidden="true">
            <div class="modal-dialog">
                <div class="modal-content">
                    <div class="modal-header">
                        <h1 class="modal-title fs-5" id="editModalLabel">Editar</h1>
                        <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                    </div>
                    <form @submit.prevent="saveEdit()">
                        <div class="modal-body" v-if="itemSelected">
                            <div class="mb-3">
                                <label for="updateName" class="form-label">Name</label>
                                <input type="text" v-model="itemSelected.name" class="form-control" id="updateName">
                            </div>
                            <div class="mb-3">
                                <label for="updateEmail" class="form-label">Email</label>
                                <input type="email" v-model="itemSelected.email" class="form-control" id="updateEmail">
                            </div>
                            <div class="mb-3">
                                <label for="updateAddress" class="form-label">Address</label>
                                <input type="text" v-model="itemSelected.address" class="form-control" id="updateAddress">
                            </div>
                            <div class="mb-3">
                                <label for="updatePhone" class="form-label">Phone</label>
                                <input type="tel" v-model="itemSelected.phone" class="form-control" id="updatePhone">
                            </div>
                            <div class="mb-3">
                                <label for="updateCountry" class="form-label">Country</label>
                                <select class="form-select" v-model="itemSelected.country" id="updateCountry">
                                    <option v-for="country in countries" :value="country">{{country}}</option>
                                </select>
                            </div>
                            <div class="mb-3">
                                <label for="updateCity" class="form-label">City</label>
                                <input type="text" v-model="itemSelected.city" class="form-control" id="updateCity">
                            </div>
                        </div>
                        <div class="modal-footer">
                            <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Cancelar</button>
                            <button type="submit" class="btn btn-primary">Guardar cambios</button>
                        </div>
                    </form>
                </div>                
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                countries: ['USA', 'Canada', 'UK', 'Australia', 'Bolivia', 'Argentina', 'Brasil'],
                newAgenda: {
                    id: null,
                    name: '',
                    email: '',
                    address: '',
                    phone: '',
                    country: '',
                    city: ''
                },
                agendaArray: [
                    {
                        id: 1,
                        name: "Alice Johnson",
                        email: "alice.johnson@example.com",
                        address: "123 Maple Street",
                        phone: "123-456-7890",
                        country: "USA",
                        city: "New York"
                    },
                    {
                        id: 2,
                        name: "Bob Smith",
                        email: "bob.smith@example.com",
                        address: "456 Oak Avenue",
                        phone: "987-654-3210",
                        country: "Canada",
                        city: "Toronto"
                    },
                    {
                        id: 3,
                        name: "Carol White",
                        email: "carol.white@example.com",
                        address: "789 Pine Road",
                        phone: "555-123-4567",
                        country: "UK",
                        city: "London"
                    },
                    {
                        id: 4,
                        name: "David Brown",
                        email: "david.brown@example.com",
                        address: "321 Elm Street",
                        phone: "444-555-6666",
                        country: "Australia",
                        city: "Sydney"
                    },
                    {
                        id: 5,
                        name: "Emily Davis",
                        email: "emily.davis@example.com",
                        address: "654 Spruce Lane",
                        phone: "333-444-5555",
                        country: "USA",
                        city: "Los Angeles"
                    }
                ],
                itemSelected: null,
                indexSelected: null,
                typeFilter: '',
                toFilter: '',
                toSearch: ''
            }
        },
        methods: {
            save(){
                // Validar que los campos no esten vacíos
                if(this.newAgenda.name && this.newAgenda.email && this.newAgenda.address && this.newAgenda.phone && this.newAgenda.country && this.newAgenda.city){
                    this.agendaArray.push({...this.newAgenda});
                    this.newAgenda.name = '';
                    this.newAgenda.email = '';
                    this.newAgenda.address = '';
                    this.newAgenda.phone = '';
                    this.newAgenda.country = '';
                    this.newAgenda.city = '';
                } else {
                    alert("Por favor, completa todos los campos");
                }
            },
            remove(index){
                console.log('presionó eliminar el índice ' + index);
                if(confirm("¿Está seguro que desea eliminar el ítem seleccionado?")) {
                    this.agendaArray.splice(index, 1);
                }
            },
            edit(index){
                console.log('presionó editar el índice ' + index);
                this.itemSelected = {...this.agendaArray[index]};
                this.indexSelected = index;
                const editModal = new bootstrap.Modal(document.getElementById('editModal'));
                editModal.show();
            },
            saveEdit(){
                if(this.itemSelected.name && this.itemSelected.email && this.itemSelected.address && this.itemSelected.phone && this.itemSelected.country && this.itemSelected.city){
                    console.log("entró al if de editar")
                    this.agendaArray[this.indexSelected] = {...this.itemSelected};

                    const modalElement = document.getElementById('editModal');
                    const modalInstance = bootstrap.Modal.getInstance(modalElement) || new bootstrap.Modal(modalElement);
                    modalInstance.hide();

                    this.itemSelected = null;
                    this.indexSelected = null;
                } else {
                    alert("Por favor, completa todos los campos");
                }
            },
            getList(){
                let result = this.agendaArray.filter((item) => {
                    if (this.toSearch) {
                        return item.name.includes(this.toSearch);
                    }
                    return true;
                });
                return result.filter((item) => {
                    if (this.toFilter) {
                        return item.name === this.toFilter;
                    }
                    // if (this.toFilter) {
                    //     return item.country === this.toFilter;
                    // }
                    return true;
                });
            },
        },
    }
</script>

<style>
    .btn {
        margin-right: 3px;
    }
    .card{
        overflow-x: auto;
    }
</style>