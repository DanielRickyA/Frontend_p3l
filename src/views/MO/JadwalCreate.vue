<template>
  <div
    class="d-flex justify-content-between flex-wrap flex-mdnowrap align-items-center pt-3 pb-2 mb-3 border-bottom"
  >
    <router-link
      :to="{ name: 'indexJadwalView' }"
      class="btn btn-md"
      id="btn-create-member"
      ><i class="fa-solid fa-arrow-left pe-2"></i>Back</router-link
    >
    <h1 class="h2">Create Jadwal</h1>
  </div>
  <div class="mt-5">
    <div class="row">
      <div class="col-md-12">
        <div class="card border-0 rounded shadow">
          <div class="card-body">
            <h3 style="text-align: left">Input Jadwal</h3>
            <form @submit.prevent="createJadwal" style="text-align: left">
              <div class="form-group mb-3">
                <label class="form-label" bis_skin_checked="1"
                  >Nama Kelas</label
                >
                <select
                  class="form-select"
                  placeholder="Masukkan nama member"
                  v-model="jadwal.id_kelas"
                  required
                >
                  <option hidden disabled selected value="">Pilih Kelas</option>

                  <option
                    v-for="kls in kelas"
                    :value="kls.id"
                    v-bind:key="kls.id"
                  >
                    {{ kls.nama }}
                  </option>
                </select>
                <!-- validation -->
                <div v-if="validation.nama" class="mt-2 alert alert-danger">
                  {{ validation.f_kelas.nama[0] }}
                </div>
              </div>
              <div class="form-group mb-3">
                <label for="content" class="form-label">Nama Instruktur</label>
                <select
                  class="form-select"
                  placeholder="Masukkan nama Instruktur"
                  v-model="jadwal.id_instruktur"
                  required
                >
                  <option hidden disabled selected value="">
                    Pilih Instruktur
                  </option>

                  <option
                    v-for="ins in instruktur"
                    :value="ins.id"
                    v-bind:key="ins.id"
                  >
                    {{ ins.nama }}
                  </option>
                </select>
                <!-- validation -->
                <div
                  v-if="validation.tanggal_lahir"
                  class="mt-2 alert alert-danger"
                >
                  {{ validation[0] }}
                </div>
              </div>

              <div class="form-group mb-3">
                <label for="content" class="form-label">Hari Kelas</label>
                <select
                  class="form-select"
                  v-model="jadwal.hari_kelas"
                  placeholder="Masukkan Hari Kelas"
                  required
                >
                  <option hidden disabled selected value="">Pilih Hari</option>
                  <option value="Monday">Monday</option>
                  <option value="Tuesday">Tuesday</option>
                  <option value="Wednesday">Wednesday</option>
                  <option value="Thursday">Thursday</option>
                  <option value="Friday">Friday</option>
                  <option value="Saturday">Saturday</option>
                  <option value="Sunday">Sunday</option>
                </select>
                <!-- validation -->
                <div v-if="validation.alamat" class="mt-2 alert alert-danger">
                  {{ validation.hari_kelas[0] }}
                </div>
              </div>
              <div class="form-group mb-3">
                <label for="content" class="form-label">Jam Kelas</label>
                <select
                  class="form-select"
                  v-model="jadwal.jam_kelas"
                  placeholder="Masukkan Jam Kelas"
                  required
                >
                  <option hidden disabled selected value="">
                    Pilih Jam Kelas
                  </option>
                  <option value="07.00 - 08.00">07.00 - 08.00</option>
                  <option value="08.00 - 09.00">08.00 - 09.00</option>
                  <option value="09.00 - 10.00">09.00 - 10.00</option>
                  <option value="10.00 - 11.00">10.00 - 11.00</option>
                  <option value="11.00 - 12.00">11.00 - 12.00</option>
                  <option value="12.00 - 13.00">12.00 - 13.00</option>
                  <option value="13.00 - 14.00">13.00 - 14.00</option>
                  <option value="14.00 - 15.00">14.00 - 15.00</option>
                  <option value="15.00 - 16.00">15.00 - 16.00</option>
                  <option value="16.00 - 17.00">16.00 - 17.00</option>
                  <option value="17.00 - 18.00">17.00 - 18.00</option>
                  <option value="18.00 - 19.00">18.00 - 19.00</option>
                  <option value="19.00 - 20.00">19.00 - 20.00</option>
                  <option value="20.00 - 21.00">20.00 - 21.00</option>
                </select>
                <!-- validation -->
                <div v-if="validation.no_telp" class="mt-2 alert alert-danger">
                  {{ validation.jam_kelas[0] }}
                </div>
              </div>
              <button type="submit" class="btn btn-primary">SIMPAN</button>
            </form>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import { onMounted, reactive, ref } from "vue";
import * as Api from "../ApiHelper";
import { useRouter } from "vue-router";
import toastr from "toastr";
import "toastr/build/toastr.min.css";

export default {
  setup() {
    const jadwal = reactive({
      id: "",
      id_kelas: "",
      id_instruktur: "",
      tanggal: "",
      hari_kelas: "",
      jam_kelas: "",
      f_kelas: {
        nama: "",
      },
      f_instruktur: {
        nama: "",
      },
    });

    const kelas = ref([]);
    const instruktur = ref([]);

    let router = useRouter();
    let validation = ref({});

    function getKelas() {
      axios
        .get(Api.BASE_URL + "/Kelas", {
          headers: {
            Accept: "application/json",
            Authorization: "Bearer " + localStorage.getItem("token"),
          },
        })
        .then((response) => {
          kelas.value = response.data.data;
          console.log(kelas);
        })
        .catch((error) => {
          console.log(error);
        });
    }

    function getInstruktur() {
      axios
        .get(Api.BASE_URL + "/Instruktur", {
          headers: {
            Accept: "application/json",
            Authorization: "Bearer " + localStorage.getItem("token"),
          },
        })
        .then((response) => {
          instruktur.value = response.data.data;
          console.log(instruktur);
        })
        .catch((error) => {
          console.log(error);
        });
    }

    function createJadwal() {
      let id_kelas = jadwal.id_kelas;
      let id_instruktur = jadwal.id_instruktur;
      let hari_kelas = jadwal.hari_kelas;
      let jam_kelas = jadwal.jam_kelas;

      axios
        .post(
          Api.BASE_URL + "/JadwalUmum",
          {
            id_kelas: id_kelas,
            id_instruktur: id_instruktur,
            hari_kelas: hari_kelas,
            jam_kelas: jam_kelas,
          },
          {
            headers: {
              Accept: "application/json",
              Authorization: "Bearer " + localStorage.getItem("token"),
            },
          }
        )
        .then(() => {
          toastr.success("Jadwal berhasil ditambahkan");
          router.push({
            name: "indexJadwalView",
          });
        })
        .catch((error) => {
          console.log(error);
          validation.value = error.response.data.message;
          toastr.error(error.response.data.message);
        });
    }

    onMounted(() => {
      getKelas();
      getInstruktur();
    });

    return {
      jadwal,
      Api,
      validation,
      kelas,
      instruktur,
      toastr,
      createJadwal,
    };
  },
};
</script>

<style>
#btn-create-member {
  background: radial-gradient(
      650px circle at 0% 0%,
      hsl(218, 41%, 35%) 15%,
      hsl(218, 41%, 30%) 35%,
      hsl(218, 41%, 20%) 75%,
      hsl(218, 41%, 19%) 80%,
      transparent 100%
    ),
    radial-gradient(
      1250px circle at 100% 100%,
      hsl(218, 41%, 45%) 15%,
      hsl(218, 41%, 30%) 35%,
      hsl(218, 41%, 20%) 75%,
      hsl(218, 41%, 19%) 80%,
      transparent 100%
    );
  color: #fff;
}

.form-label {
  color: black;
  font-weight: medium;
}
</style>
