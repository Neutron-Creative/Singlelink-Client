<template>
  <section class="flex flex-shrink-0 flex-col p-8 items-center bg-gray-100 overflow-x-hidden overflow-y-scroll">
    <h1 class="text-gray-800 font-extrabold tracking-tight text-3xl w-full mb-4">
      Site settings
    </h1>
    <div class="flex flex-col p-6 bg-white shadow rounded-lg w-full mb-8">
      <transition name="fade">
        <div
          v-if="error"
          class="flex flex-row p-2 mb-4 bg-orange-200 text-orange-600 rounded-lg w-full justify-center items-center text-sm border border-orange-300 shadow-sm"
        >
          <img style="width: 12px;" src="/caution.svg" alt="caution">
          <div class="flex flex-col ml-2">
            {{ error }}
          </div>
        </div>
      </transition>

      <h2 class="text-gray-800 font-semibold text-lg w-full mb-2">
        Details
      </h2>
      <form class="flex flex-col">
        <div class="flex flex-col lg:flex-row mb-3">
          <div class="flex flex-col w-full lg:w-1/2 mr-4 mb-3 lg:mb-0">
            <label class="font-medium text-sm text-gray-800" for="name">Headline</label>
            <input
              id="name"
              v-model="user.activeProfile.headline"
              class="p-2 mt-2 text-sm border-solid border-gray-300 rounded-lg border"
              type="text"
              placeholder="e.g. Jane Doe, 21"
            >
          </div>
          <div class="flex flex-col w-full lg:w-1/2">
            <label class="font-medium text-sm text-gray-800" for="subtitle">Subtitle</label>
            <input
              id="subtitle"
              v-model="user.activeProfile.subtitle"
              class="p-2 mt-2 text-sm border-solid border-gray-300 rounded-lg border"
              type="text"
              placeholder="e.g. Developer at Neutron from Raleigh NC"
            >
          </div>
        </div>
        <div class="flex flex-col lg:flex-row mb-4">
          <div class="flex flex-col w-full lg:w-1/2 mr-3 mb-3 lg:mb-0">
            <label class="font-medium text-sm text-gray-800" for="handle">Handle</label>
            <div class="flex flex-row rounded-lg border border-solid border-gray-300 text-sm mt-2 overflow-hidden">
              <span
                class="flex p-2 bg-gray-100 border text-gray-700 border-solid border-gray-300 border-t-0 border-l-0 border-b-0"
              >{{ hostname }}/u/</span>
              <input
                id="handle"
                v-model="user.activeProfile.handle"
                class="p-2 flex-grow"
                type="text"
                placeholder="e.g. janedoe"
                autocomplete="off"
              >
            </div>
          </div>
          <div class="flex flex-col w-full lg:w-1/2">
            <label class="font-medium text-sm text-gray-800" for="visibility">Visibility</label>
            <select
              id="visibility"
              v-model="user.activeProfile.visibility"
              class="p-2 mt-2 text-sm border-solid border-gray-300 rounded-lg border"
            >
              <option value="unpublished">
                Unpublished, not viewable
              </option>
              <option value="published">
                Public, no sensitive content (Most used)
              </option>
              <option value="published-18+">
                Public, sensitive content warning
              </option>
            </select>
          </div>
        </div>
        <div class="flex flex-row items-center justify-center space-x-4 mb-4">
          <input type="hidden" name="avatar_url" id="avatar_url" class="simple-file-upload" v-model="user.activeProfile.imageUrl">
          <div class="flex flex-col w-full">
          <label class="font-medium text-sm text-gray-800" for="image_url">Avatar Image URL</label>
          <input
            id="image_url"
            v-model="user.activeProfile.imageUrl"
            class="p-2 mt-2 text-sm border-solid border-gray-300 rounded-lg border"
            type="text"
            placeholder="e.g. https://uifaces.co/our-content/donated/rSuiu_Hr.jpg"
          >
          <div
            v-if="!profile_valid"
            class="py-3 px-4 rounded-lg bg-red-200 border border-red-400 text-red-500 flex flex-col items-start mt-2 text-sm"
          >
            <span class="font-semibold">Warning!</span>
            <span class="text-xs font-medium">Your site picture may be improperly formatted! Please ensure your image is loaded via an SSL and ends in .gif, .png, .jpg, .jpeg, or another supported file extension.<a
              href="https://www.notion.so/neutroncreative/Troubleshooting-9a162db4a8ce482d89b3d3e1bc9825ba"
              target="_blank"
              class="ml-2 font-semibold underline hover:text-red-700"
            >Learn more</a></span>
          </div>
        </div>
        </div>

        <div class="flex flex-col w-full mb-6">
          <div class="flex flex-col lg:flex-row space-y-1 lg:space-y-0 items-start lg:justify-between lg:items-center w-full">
          <label class="font-medium text-sm text-gray-800" for="custom_domain">Custom domain</label>
            <a href="https://www.notion.so/neutroncreative/Setting-up-your-custom-domain-907421b1ac3841dbbd8d9a7d41d17f9a" class="text-gray-500 text-xs hover:underline hover:text-gray-600">Need help? Read our documentation</a>
          </div>
          <input
            id="custom_domain"
            v-model="user.activeProfile.customDomain"
            class="p-2 mt-2 text-sm border-solid border-gray-300 rounded-lg border"
            type="text"
            placeholder="e.g. neutroncreative.com (no http/https)"
          >
        </div>

        <!-- Watermark Toggle -->
        <div class="flex flex-row w-full mb-6 items-start">
          <input
            id="themeGlobal"
            v-model="user.activeProfile.showWatermark"
            type="checkbox"
            style="margin-top:3px;"
            class="form-checkbox h-4 w-4 text-indigo-600 transition duration-150 ease-in-out"
          >

          <label
            for="themeGlobal"
            class="ml-4 flex font-medium text-sm leading-5 text-gray-600 w-full lg:w-auto flex-col"
            style="max-width:calc(100% - 32px)"
          >
            Display Watermark ("Proudly built with {{ app_name }}!")
            <br>
            <span
              v-show="showWatermarkNotice"
              class="mt-1 flex text-gray-800 font-semibold text-xs lg:text-sm"
            >
              This is completely optional, but it really helps us out! Mind spreading the word about {{ app_name }}?
            </span>
          </label>
        </div>

        <!-- Privacy mode toggle -->
        <div class="flex flex-row w-full mb-6 items-start">
          <input
            v-model="user.activeProfile.metadata.privacyMode"
            type="checkbox"
            style="margin-top:3px;"
            class="form-checkbox h-4 w-4 text-indigo-600 transition duration-150 ease-in-out"
            aria-label="privacy mode"
          >

          <label class="ml-4 block text-sm leading-5 text-gray-600">
            Privacy mode (Disables site analytics & discovery)
          </label>
        </div>

        <button
          type="button"
          class="mt-2 inline-flex p-3 text-sm text-white text-center bg-indigo-600 hover:bg-indigo-700 rounded-lg font-semibold w-auto max-w-xs justify-center align-center"
          @click="saveChanges"
        >
          Save changes
        </button>
      </form>
    </div>

    <!-- Delete site -->
    <div class="flex flex-col lg:flex-row p-6 bg-white shadow rounded-lg justify-center items-center w-full mb-8">
      <div class="flex flex-col mr-auto w-full lg:w-1/2">
        <h2 class="text-gray-800 font-semibold text-lg w-full">
          Delete this site
        </h2>
        <p class="text-gray-600 font-medium">Done with this site? Click the button on your right to delete this
          site and all related content.</p>
      </div>
      <button
        type="button"
        class="w-full lg:w-auto mt-4 lg:mt-0 ml-2 flex p-3 text-sm text-white text-center bg-red-600 hover:bg-red-700 rounded-lg font-semibold w-1/3 justify-center align-center"
        @click="setDeleteProfileModalActive(true)"
      >
        Delete this site
      </button>
    </div>

    <transition name="fade">
      <!-- Confirm site deletion modal -->
      <div
        v-if="deleteProfileModalActive"
        class="w-screen h-screen absolute top-0 left-0 right-0 bottom-0 z-50 flex items-center justify-center"
        style="background: rgba(0,0,0,.5); backdrop-filter: saturate(180%) blur(5px);"
        @click="setDeleteProfileModalActive(false)"
      >
        <div class="flex flex-col p-6 bg-white shadow rounded-lg w-full max-w-lg" @click.stop>
          <h2 class="text-gray-800 font-semibold text-xl">
            Are you sure?
          </h2>
          <p class="text-gray-600 text-sm">
            Deleting this site is irreversible, please confirm to continue.
          </p>
          <button
            type="button"
            class="mt-4 w-full p-4 text-center text-md text-white bg-red-600 hover:bg-red-700 rounded-lg font-semibold"
            @click="deleteProfile"
          >
            Yes, delete this site
          </button>
        </div>
      </div>
    </transition>

    <transition name="fade">
      <!-- Password reset confirmation modal -->
      <div
        v-if="resetPasswordModalActive"
        class="w-screen h-screen absolute top-0 left-0 right-0 bottom-0 z-50 flex items-center justify-center"
        style="background: rgba(0,0,0,.5); backdrop-filter: saturate(180%) blur(5px);"
        @click="setPasswordModalActive(false)"
      >
        <div class="flex flex-col p-6 bg-white shadow rounded-lg w-full max-w-lg" @click.stop>
          <h2 class="text-gray-800 font-semibold text-xl">
            {{ passwordError ? "Error on password request!" : "Password reset requested" }}
          </h2>
          <p v-if="!passwordError" class="text-gray-600 text-sm">A password reset link has been sent to your account
            email inbox successfully.
            Make sure to check your spam folder.</p>

          <p v-if="passwordError" class="text-gray-600 text-sm">
            <i class="fas fa-exclamation-triangle"/>
            {{ passwordError }}
          </p>
          <button
            type="button"
            class="mt-4 p-3 text-center text-md text-white bg-indigo-600 hover:bg-indigo-700 rounded-lg font-semibold"
            @click="setPasswordModalActive(false)"
          >
            Close
          </button>
        </div>
      </div>
    </transition>

  </section>
</template>

<script lang="ts">
import crypto from "crypto";
import Vue from "vue";
import {StatusCodes} from "http-status-codes";

export default Vue.extend({
  name: 'DashboardSettings',
  layout: 'dashboard',
  middleware: 'authenticated',
  head: {
    title: 'Site settings - ' + process.env.APP_NAME,
    meta: [
      {
        hid: 'description',
        name: 'description',
        content: 'Take administrative control over your microsites through the settings panel.'
      },
      {
        hid: 'twitter:description',
        name: 'twitter:description',
        content: 'Take administrative control over your microsites through the settings panel.'
      },
      {
        hid: 'og:title',
        name: 'og:title',
        content: 'Site settings - ' + process.env.APP_NAME
      },
      {
        hid: 'twitter:title',
        name: 'twitter:title',
        content: 'Site settings - ' + process.env.APP_NAME
      },
      {
        hid: 'og:description',
        name: 'og:description',
        content: 'Take administrative control over your microsites through the settings panel.'
      },
    ],
  },
  data() {
    return {
      loaded: false,
      resetPasswordModalActive: false,
      deleteProfileModalActive: false,
      originalHandle: '',
      user: {
        name: '',
        emailHash: '',
        activeProfile: {
          imageUrl: '',
          headline: '',
          subtitle: '',
          handle: '',
          customDomain: '',
          visibility: '',
          showWatermark: false,
          metadata: {
            privacyMode: false
          },
        }
      },
      error: '',
      passwordError: '',
      passwordEmail: '',
      showWatermarkNotice: false,
      hostname: process.env.HOSTNAME,
      app_name: process.env.APP_NAME,
      icon_url: process.env.ICON_URL
    };
  },

  computed: {
    profile_valid() {
      if (!this.user.activeProfile.imageUrl) {
        return true;
      }

      if (!this.user.activeProfile.imageUrl.includes('.jpg') && !this.user.activeProfile.imageUrl.includes('.jpeg') && !this.user.activeProfile.imageUrl.includes('.png') && !this.user.activeProfile.imageUrl.includes('gif')) {
        return false;
      }

      if (!this.user.activeProfile.imageUrl.includes('https://')) {
        return false;
      }

      return true;
    }
  },

  watch: {
    'user.activeProfile.showWatermark': {
      handler(val) {
        this.showWatermarkNotice = (!val && this.loaded);
      }
    }
  },

  async mounted() {
    await this.getUserData();

    this.loaded = true;
  },

  methods: {
    async getUserData() {
      try {
        const token = this.$store.getters['auth/getToken'];

        const userResponse = await this.$axios.$post('/user', {
          token
        });

        const profileResponse = await this.$axios.$post('/profile/active-profile', {
          token
        });

        this.user.name = userResponse.name;
        this.user.emailHash = userResponse.emailHash;

        this.user.activeProfile.imageUrl = profileResponse.imageUrl;
        this.user.activeProfile.headline = profileResponse.headline;
        this.user.activeProfile.subtitle = profileResponse.subtitle;
        this.user.activeProfile.handle = profileResponse.handle;
        this.user.activeProfile.customDomain = profileResponse.customDomain;
        this.user.activeProfile.visibility = profileResponse.visibility;
        this.user.activeProfile.showWatermark = profileResponse.showWatermark;

        this.user.activeProfile.metadata.privacyMode = profileResponse.metadata?.privacyMode ?? false;

        this.$set(this.user.activeProfile, 'user.activeProfile', profileResponse);

        this.originalHandle = this.user.activeProfile.handle;
      } catch (err) {
        console.log('Error getting user data');
        console.log(err);
      }
    },

    async saveChanges() {
      // Update profile
      try {

        let avatar_upload = (<HTMLInputElement>document.getElementById('avatar_url'));
        let avatar_string = null;
        if(avatar_upload && avatar_upload.value) {
          avatar_string = avatar_upload.value;
        }

        await this.$axios.$post('/profile/update', {
          token: this.$store.getters['auth/getToken'],
          imageUrl: avatar_string ?? this.user.activeProfile.imageUrl ?? null,
          headline: this.user.activeProfile.headline ?? null,
          subtitle: this.user.activeProfile.subtitle ?? null,
          handle: this.user.activeProfile.handle ?? null,
          visibility: this.user.activeProfile.visibility ?? null,
          customDomain: this.user.activeProfile.customDomain ?? null,
          showWatermark: this.user.activeProfile.showWatermark ?? true,
        });

        if (process.client) {
          if (this.user.activeProfile.handle !== this.originalHandle) {
            location.reload();
            return;
          }

          this.$root.$emit('refreshUserProfileView');
        }
      } catch (err) {
        if (err.response) {
          if (err.response.status === StatusCodes.CONFLICT) {
            console.error("This handle is already being used by another profile.");
            this.error = "This handle is already being used by another profile.";

            return;
          }
        }

        throw err;
      }

      // Update privacy mode
      const privacyMode = this.user.activeProfile.metadata.privacyMode;

      try {
        const request = await this.$axios.post('/profile/set-privacy-mode', {
          token: this.$store.getters['auth/getToken'],
          privacyMode
        });

        if (request.status && request.status === 200) {
          this.passwordError = '';
        }
      } catch (err) {
        console.error(err);

        if (err.response) {
          if (err.response.status === StatusCodes.NOT_FOUND) {
            // This should be impossible under normal circumstances
            this.error = "The profile couldn't be found, please make sure it's correct.";
          }

          return;
        }

        throw err;
      }
    },

    setPasswordModalActive(active: boolean) {
      this.resetPasswordModalActive = active;

      if (active) {
        if (!this.passwordEmail) {
          this.passwordError = "Please enter a valid email.";
          return;
        } else {
          this.passwordError = '';
        }

        const md5 = crypto.createHash('md5').update(this.passwordEmail).digest('hex');

        console.log(md5);
        console.log(this.user.emailHash);

        if (md5 !== this.user.emailHash) {
          this.passwordError = "Please enter the same email you used for this account.";
          return;
        }
        this.requestPasswordReset();
      }
    },

    setDeleteProfileModalActive(active: boolean) {
      this.deleteProfileModalActive = active;
    },

    async deleteProfile() {
      this.$nuxt.$loading.start();

      await this.$axios.$post('/profile/delete', {
        token: this.$store.getters['auth/getToken']
      });

      this.$nuxt.$loading.finish();

      location.reload();
    },

    async requestPasswordReset() {
      try {
        const request = await this.$axios.post('/user/request-reset-password', {
          email: this.passwordEmail
        });
        if (request.status && request.status === 200) {
          this.passwordError = '';
        }
      } catch (err) {
        console.error(err);

        this.passwordError = err.toString();

        if (err.response) {
          if (err.response.status === StatusCodes.NOT_FOUND) {
            this.passwordError = "The email couldn't be found, please make sure it's correct.";
          }

          if (err.response.status === StatusCodes.TOO_MANY_REQUESTS) {
            this.passwordError = `Whoa, slow down! Error: ${err.response.data.message}`;
          }

          return;
        }

        throw err;
      }
    },
  }
});
</script>

<style lang="scss">
.fade-enter-active, .fade-leave-active {
  transition: opacity .25s;
}

.fade-enter, .fade-leave-to {
  opacity: 0;
}

* {
  outline: none !important;
}
</style>
