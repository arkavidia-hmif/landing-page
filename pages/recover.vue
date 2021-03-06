<template>
  <v-container fill-height class="py-12 px-4" style="min-height: 100vh">
    <v-row class="mt-8 mb-12">
      <v-col :cols="12" :lg="4" :md="5">
        <div class="headline font-weight-bold section-title">
          Lupa kata sandi
        </div>
        <Alert v-if="error" type="error" class="mt-4" :message="error" />
        <template v-if="!emailSent">
          <div class="mt-4 grey--text text--darken-1">
            Jangan khawatir! Masukkan alamat e-mail kamu, nanti kami akan mengirimkan e-mail untuk membuat kata sandi baru.
          </div>
          <form @submit.prevent="attemptRecover">
            <v-text-field
              v-model="email"
              label="Alamat E-mail"
              class="mt-4"
              outlined
              dense
            />
            <v-btn class="mt-1 text-none" type="submit" block outlined :loading="isRecovering">
              Kirim
            </v-btn>
          </form>
        </template>
        <template v-else>
          <v-chip
            color="green"
            text-color="white"
            class="mt-4"
          >
            <v-avatar left>
              <v-icon>mdi-checkbox-marked-circle</v-icon>
            </v-avatar>
            E-mail Terkirim
          </v-chip>
          <p class="mt-4">
            Kami akan mengirimkan e-mail ke {{ email }}, apabila terdaftar.
            Silakan buka tautan yang ada pada e-mail tersebut dan ikuti instruksi selanjutnya.
          </p>
          <v-btn outlined block class="text-none mt-2" to="/login">
            Halaman Login
          </v-btn>
        </template>
      </v-col>
    </v-row>
  </v-container>
</template>

<script lang="ts">
import { Component, Vue, Action } from 'nuxt-property-decorator';
import Alert from '~/components/partials/Alert.vue';
@Component({
  components: { Alert }
})
export default class DashboardLogin extends Vue {
  isRecovering: boolean = false;
  emailSent: boolean = false;
  email: string = '';
  error: string = '';

  @Action('user/recover') recoverAction;

  head() {
    return {
      title: 'Lupa kata sandi'
    };
  }

  attemptRecover() {
    if (!this.validateEmail(this.email)) {
      this.error = 'Alamat e-mail salah';
      return;
    }

    this.isRecovering = true;
    this.error = '';

    this.recoverAction({ email: this.email })
      .then(() => {
        this.emailSent = true;
      })
      .catch((e) => {
        this.error = e.toString();
      })
      .finally(() => {
        this.isRecovering = false;
      });
  }

  validateEmail(email): boolean {
    // eslint-disable-next-line no-useless-escape
    const re = /^(([^<>()\[\]\\.,;:\s@"]+(\.[^<>()\[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;
    return re.test(String(email).toLowerCase());
  }
}
</script>

<style scoped>
  .section-title {
    position: relative;
    padding-bottom: 8px;
  }

  .section-title:after {
    content: "";
    position: absolute;
    width: 100px;
    height: 1px;
    bottom: 0;
    left: 0;
    border-bottom: 4px solid #FF084F;
  }
</style>
