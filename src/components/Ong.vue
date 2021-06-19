<template>
    <v-data-table 
    :headers="headers"
    :items="ongs"
    :search="search"
    sort-by="institucion"
    class="elevation-1" style="width:1300px">
        <template v-slot:top>
            <v-toolbar flat color="white">
                <v-toolbar-title>DATASET - ONG LIST</v-toolbar-title>
                <v-divider class="mx-4" inset vertical />
                <v-spacer></v-spacer>
                <v-text-field label="Search ONG" append-icon="search" 
                class="text-xs-center" v-model="search" single-line hide-details></v-text-field>
                <v-spacer></v-spacer>
                <v-dialog v-model="dialog" max-width="500px">
                    <template v-slot:activator="{ on }">
                        <v-btn color="primary" dark class="mb-2" v-on="on">New ONG</v-btn>
                    </template>
                    <v-card>
                        <v-card-title>
                            <span class="headline">{{ formTitle }}</span>
                        </v-card-title>

                        <v-card-text>
                            <v-container>
                                <v-row>
                                    <v-col cols="12" sm="12" md="12">
                                        <v-text-field v-model="institucion" label="Institucion"></v-text-field>
                                    </v-col>
                                    <v-col cols="12" sm="12" md="12">
                                        <v-text-field v-model="departamento" label="Departamento"></v-text-field>
                                    </v-col>
                                    <v-col cols="12" sm="12" md="12">
                                        <v-text-field v-model="provincia" label="Provincia"></v-text-field>
                                    </v-col>
                                    <v-col cols="12" sm="12" md="12">
                                        <v-text-field v-model="distrito" label="Distrito"></v-text-field>
                                    </v-col>
                                    <v-col cols="12" sm="12" md="12">
                                        <v-text-field v-model="representante" label="Representante"></v-text-field>
                                    </v-col>
                                    <v-col cols="12" sm="12" md="12">
                                        <v-text-field v-model="sector" label="Sector"></v-text-field>
                                    </v-col>
                                </v-row>
                            </v-container>
                        </v-card-text>
                        <v-card-actions>
                            <v-spacer></v-spacer>
                            <v-btn color="blue darken-1" text @click="close">Cancel</v-btn>
                            <v-btn color="blue darken-1" text @click="save">Save</v-btn>
                        </v-card-actions>
                    </v-card>
                </v-dialog>
            </v-toolbar>
        </template>
        <template v-slot:item="{ item }">
            <tr>
                <td>{{ item.institucion }}</td>
                <td>{{ item.departamento }}</td>
                <td>{{ item.provincia }}</td>
                <td>{{ item.distrito }}</td>
                <td>{{ item.representante }}</td>
                <td>{{ item.sector }}</td>
            </tr>
        </template>

        <template v-slot:no-data>
            <v-btn color="primary" @click="listOngs">Reset</v-btn>
        </template>
    </v-data-table>
</template>

<script>
    import axios from 'axios'
    
    export default {
        data:() => ({
            search: '',
            numero: '',
            institucion: '',
            departamento: '',
            provincia: '',
            distrito: '',
            representante: '',
            sector: '',
            dialog: false,
            ongs: [],
            valid: 0,
            validMessage: [],
            headers: [
                { text: 'Institucion', value: 'institucion', sortable: true },
                { text: 'Departamento', value: 'departamento', sortable: false },
                { text: 'Provincia', value: 'provincia', sortable: false },
                { text: 'Distrito', value: 'distrito', sortable: false },
                { text: 'Representante', value: 'representante', sortable: false },
                { text: 'Sector', value: 'sector', sortable: false },
            ]
        }),
        computed: {
            formTitle() {
                return'New Ong';
            }
        },
        watch: {
            dialog (val) {
                val || this.close()
            }
        },
        created() {
            this.listOngs();
        },
        methods: {
            listOngs() {
                let me= this;
                axios.get('ongs').then(function(response){
                    me.ongs = response.data;
                }).catch(function(error){
                    console.log(error);
                })
            },
            close() {
                this.dialog = false;
            },
            clean() {
                this.numero = "";
                this.institucion = "";
                this.departamento = "";
                this.provincia= "";
                this.distrito = "";
                this.representante = "";
                this.sector = "";
            },
            save() {
                let me=this;
                axios.post('ongs', {
                    'numero': me.numero,
                    'institucion': me.institucion,
                    'departamento': me.departamento,
                    'provincia': me.provincia,
                    'distrito': me.distrito,
                    'representante': me.representante,
                    'sector': me.sector,
                }).then(function(response){
                    me.close();
                    me.listOngs();
                    me.clean();
                }).catch(function(error) {
                    console.log(error);
                });
            }
        }
    }
</script>