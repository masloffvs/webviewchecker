<template>
  <div id="app" class="bg-gray-50">
    <AppsFlyerError v-if="errorCode == 'ERROR_CODE_APPSFLYER_GET_MISSING'" class="mx-4 mt-16 lg:mx-32 lg:mt-24"/>
    <RedirectInsideFirstWeb  v-if="errorCode == 'REDIRECT_INSIDE'" class="mx-4 mt-16 lg:mx-32 lg:mt-24"/>
    <TryRotateScreen v-if="challenge == 'challenge-orientation'" class="mx-4 mt-16 lg:mx-32 lg:mt-24"/>
    <ChooseFile :onFileChoose="onFileChoose" v-if="challenge == 'challenge-file-choose'" class="mx-4 mt-16 lg:mx-32 lg:mt-24"/>
    <CameraAndMic v-if="challenge == 'challenge-mic-view' || challenge == 'challenge-camera-view'" class="mx-4 mt-16 lg:mx-32 lg:mt-24"/>
    <CanGoBack v-if="challenge == 'challenge-back'" class="mx-4 mt-16 lg:mx-32 lg:mt-24"/>
    <DoneView v-if="challenge == 'challenge-done'" class="mx-4 mt-16 lg:mx-32 lg:mt-24" />

    <div class="flex justify-center content-center items-center w-full h-screen" v-else-if="challenge == 'challenge-cookie'">
      <div class="flex justify-center flex-col items-center	">  
        <div class="mb-4"  v-if="fail">   
          <div class="mx-auto flex-shrink-0 flex items-center justify-center h-12 w-12 rounded-full bg-red-100 sm:mx-0 sm:h-10 sm:w-10">
            <!-- Heroicon name: outline/exclamation -->
            <svg class="h-6 w-6 text-red-600" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor" aria-hidden="true">
              <path stroke-linecap="round" stroke-linejoin="round" d="M12 9v2m0 4h.01m-6.938 4h13.856c1.54 0 2.502-1.667 1.732-3L13.732 4c-.77-1.333-2.694-1.333-3.464 0L3.34 16c-.77 1.333.192 3 1.732 3z" />
            </svg>
          </div>
        </div>

        <transition name="slide-fade" mode="out-in">
          <div class="text-center p-5">
            <h2 class="text-base text-indigo-600 font-semibold tracking-wide uppercase">
              {{ challengeType }}
            </h2>
            <p class="mt-2 text-3xl leading-8 font-extrabold tracking-tight text-gray-900 sm:text-4xl">
              {{ intent }}
            </p>
            <p class="mt-4 max-w-2xl text-xl text-gray-500 lg:mx-auto">
              {{ description }}
            </p>
          </div>
        </transition>

        <div v-show="challenge == 'challenge-file-choose'">
          <input type="file" id="file-selector" ref="fileSelector">
        </div>
      </div>
    </div>

    <button v-if="previousState && challenge != 'challenge-done'" @click.prevent="challenge = previousState; nextChallenge(); previousState = null" type="button" class="absolute bottom-0 w-full items-center items-center px-4 py-2 border border-transparent rounded-md shadow-sm text-sm font-medium text-white bg-indigo-600 hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500">
      Начать с контрольной точки
    </button>
  </div>
</template>

<style>
  @tailwind base;
  @tailwind components;
  @tailwind utilities;
</style>

<script>
import AppsFlyerError from "@/components/AppsFlyerError";
import RedirectInsideFirstWeb from "@/components/RedirectInsideFirstWeb";
import TryRotateScreen from "@/components/TryRotateScreen";
import ChooseFile from "@/components/ChooseFile";
import CameraAndMic from "@/components/CameraAndMic";
import CanGoBack from "@/components/CanGoBack";
import getBrowserFingerprint from 'get-browser-fingerprint';

import axios from "axios";
import DoneView from "@/components/Done";

const EXTERNAL_HOST = require("./conf").REDIRECT_URL

const CHALLENGE_REDIRECT = "challenge-redirect"
const CHALLENGE_ORIENTATION = "challenge-orientation"
const CHALLENGE_FILE_CHOOSE = "challenge-file-choose"
const CHALLENGE_CAMERA_VIEW = "challenge-camera-view"
const CHALLENGE_MIC_VIEW = "challenge-mic-view"

const CHALLENGE_CAN_GO_BACK = "challenge-back"
const CHALLENGE_COOKIE = "challenge-cookie"
const CHALLENGE_DONE = "challenge-done"

const ERROR_CODE_APPSFLYER_GET_MISSING = "ERROR_CODE_APPSFLYER_GET_MISSING"
const REDIRECT_INSIDE = "REDIRECT_INSIDE"

export default {
  name: 'App',
  components: {
    DoneView,
    CanGoBack,
    AppsFlyerError,
    TryRotateScreen,
    ChooseFile,
    RedirectInsideFirstWeb,
    CameraAndMic
  },
  data() {
    return {
      errorCode: null,
      challenge: CHALLENGE_REDIRECT,
      challengeSubState: 0,
      intent: "",
      fail: false,
      currentScreenOrientation: null,
      lock: false,
      fingerprint: getBrowserFingerprint(),
      previousState: null
    }
  },
  computed: {
    description() {
      if (this.challenge == CHALLENGE_REDIRECT) {
        return "Для того чтобы не застрять на этом этапе, проверьте, передаете ли вы информацию из AppsFlyer в функцию webview.loadUrl()"
      
      } else if (this.challenge == CHALLENGE_ORIENTATION) {
        return "Теперь попробуйте перевернуть устройство, не забудьте включить автоповорот. А если вдруг тест начнется снова, то вы неправильно настроили манифест своего приложения"

      } else if (this.challenge == CHALLENGE_FILE_CHOOSE) {
        return "Вам нужно будет выбрать файл с вашего телефона. В случае если после нажатия на кнопку ничего не происходит, возможно вы забыли настроить выборщик файлов в своем WebView."

      } else if (this.challenge == CHALLENGE_CAMERA_VIEW) {
        return "На протяжении нескольких секунд мы запросим у вас доступ к камере, Вас снимать не будем :). Нам лишь нужно убедиться, что камера в вашем WebView полностью исправна. Если после 10 секунд ничего не происходит — вы забыли настроить права к камере в вашем WebView."

      } else if (this.challenge == CHALLENGE_MIC_VIEW) {
        return "Мы сделаем тоже самое, что и с камерой, только уже с микрофоном"

      } else if (this.challenge == CHALLENGE_COOKIE) {
       return "Обязательно нужно попробовать сохранить куки"

      } else if (this.challenge == CHALLENGE_CAN_GO_BACK) {
       return "Если после нажатия кнопки \"Назад\" приложение вылетело - вы не настроили коллбек на эту кнопку"

      } else if (this.challenge == CHALLENGE_DONE) {
       return "Попробуй перезапустить приложение. Если WebView откроется на этом экране, это значит, что ты все сделал правильно. Если тебя WebView попросит тебя пройти тест заново - у тебя неправильно работают куки"

      } else {
        return ""
      }
    },
    challengeType() {
      if (this.challenge == CHALLENGE_REDIRECT) {
        return "Редирект"
      
      } else if (this.challenge == CHALLENGE_ORIENTATION) {
        return "Поворот экрана"

      } else if (this.challenge == CHALLENGE_FILE_CHOOSE) {
        return "Выбора файлов"

      } else if (this.challenge == CHALLENGE_CAMERA_VIEW) {
        return "Доступ к камере"

      } else if (this.challenge == CHALLENGE_MIC_VIEW) {
        return "Доступ к микрофону"

      } else if (this.challenge == CHALLENGE_CAN_GO_BACK) {
       return "Навигация"

      } else if (this.challenge == CHALLENGE_COOKIE) {
       return "Cookie"

      } else {
        return ""
      }
    }
  },

  methods: {
    onFileChoose() {
      if (this.challenge == CHALLENGE_FILE_CHOOSE) {
         location.hash = "cameraView"
        this.challenge = CHALLENGE_CAMERA_VIEW
        this.nextChallenge()
      }
    },

    getOrientation() {
      return (screen.orientation || {}).type || screen.mozOrientation || screen.msOrientation;
    },

    nextChallenge() {
      if (this.lock) return

      axios.get("https://jvm_metric.wfc.su/api/application/ApplicationMaidenFlight/setLastState?fingerprint=" + this.fingerprint + "&state=" + this.challenge)

      switch (this.challenge) {
        case CHALLENGE_REDIRECT:
          if ((location.search == "" || location.search == "?")) {
            this.errorCode = ERROR_CODE_APPSFLYER_GET_MISSING

          } else if (location.pathname == "/" || location.pathname == "") {
            location.href = "/redirect" + location.search

          } else if (location.pathname == "/redirect") {
            this.errorCode = REDIRECT_INSIDE

            if (location.pathname != '/externalRedirect') {
              setTimeout(() => {
                location.href = EXTERNAL_HOST + "/externalRedirect" + location.search
              }, 2000);
            }

          } else if (location.pathname == '/externalRedirect') {
            this.challenge = CHALLENGE_ORIENTATION
            this.nextChallenge()

          }

          break;

        case CHALLENGE_ORIENTATION:          
          screen.orientation.addEventListener('change', () => {

            if (this.currentScreenOrientation != this.getOrientation()) {
              if (this.challengeSubState == 1) {
                location.hash = "fileChoose"
                this.challenge = CHALLENGE_FILE_CHOOSE
                this.nextChallenge()
              } else {
                this.challengeSubState = 1
              }
            }

          });

          break;

        case CHALLENGE_FILE_CHOOSE:
          break;

        case CHALLENGE_CAMERA_VIEW:
          this.intent = "Дай резрешение на съемку видео"

          var video = document.createElement('video');

          video.setAttribute('playsinline', '');
          video.setAttribute('autoplay', '');
          video.setAttribute('muted', '');
          video.style.width = '200px';
          video.style.height = '200px';

          var facingMode = "user"; // Can be 'user' or 'environment' to access back or front camera (NEAT!)
          var constraints = {
            audio: false,
            video: {
              facingMode: facingMode
            }
          };

          /* Stream it to video element */
          navigator.mediaDevices.getUserMedia(constraints)
            .catch(() => {
              this.intent = "Разрешение не получено"
              this.fail = true
              this.lock = true
            })
            .then((stream) => {
              video.srcObject = stream;

              location.hash = "microphoneView"
              this.challenge = CHALLENGE_MIC_VIEW
              this.nextChallenge()
            });

          break;

        case CHALLENGE_MIC_VIEW:
          this.intent = "Дай резрешение на запись аудио"

          navigator.mediaDevices.getUserMedia({ audio: true })
              .catch(() => {
                this.intent = "Разрешение не получено"
                this.fail = true
                this.lock = true
              })
              .then(stream => {
                const mediaRecorder = new MediaRecorder(stream);
                mediaRecorder.start();

                location.hash = "canGoBack"
                this.challenge = CHALLENGE_CAN_GO_BACK
                this.nextChallenge()
              });

          break;

        case CHALLENGE_CAN_GO_BACK:
          history.pushState({state: 1}, "Fake record 1", "?fake=1")
          history.pushState({state: 1}, "Fake record 2", "?date=1")

          this.intent = "Попробуй нажать кнопку 'Назад'"
          var previousState = 0

          window.addEventListener('popstate', (event) => {
            if (event.state > previousState) {
              this.intent = "Совершена навигация вперед, а не назад"
              this.fail = true
            } else {
              location.hash = "cookies"
              this.challenge = CHALLENGE_COOKIE
              this.nextChallenge()

              history.deleteAll()
            }
            previousState = event.state
          })
          
          history.pushState({state: 1}, "Fake record 3", "?test=1")
          history.pushState({state: 1}, "Fake record 4", "?popup=1")

          break;

        case CHALLENGE_COOKIE:
          if (navigator.cookieEnabled) {
            this.intent = "Cookie успешно включены"

            setTimeout(() => {
              this.challenge = CHALLENGE_DONE
              this.nextChallenge()
            }, 2000);
          } else {
            this.intent = "Cookie не включены"
            this.fail = true
          }

          document.cookie = "username=John Doe; expires=Thu, 18 Dec 2030 12:00:00 UTC";

          break;

        case CHALLENGE_DONE:
          this.intent = "Тест прошел успешно!"
          break;

        default:
          break;
      }
    }
  },

  /**
   * Get last session before mount to screen
   */
  beforeMount() {
    axios
      .get("https://jvm_metric.wfc.su/api/application/ApplicationMaidenFlight/getLastState?fingerprint=" + this.fingerprint)
      .then(e => {
        if(typeof e.data.lastState != 'undefined') {
          this.previousState = e.data.lastState
        }
      })

    axios
      .get("https://jvm_metric.wfc.su/api/application/ApplicationIDPassport/connectDevice?fingerprint=" + this.fingerprint + "&deviceName=" + "Device@" + this.fingerprint)
  },

  mounted() {
    if (document.cookie.match("username") != null) {
      location.href = EXTERNAL_HOST + "/externalRedirect#cookies" + location.search
      this.challenge = CHALLENGE_DONE
    } else if (document.cookie.match("username") == null && location.hash == "#cookies") {
      this.intent = "куки не сохранились"
    } else {
      if (location.pathname == '/externalRedirect' && (location.hash == "" || location.hash == "#")) {
        this.challenge = CHALLENGE_ORIENTATION
      } else if (location.hash == "#fileChoose") {
        this.challenge = CHALLENGE_FILE_CHOOSE
      } else if (location.hash == "#cameraView") {
        this.challenge = CHALLENGE_CAMERA_VIEW
      } else if (location.hash == "#microphoneView") {
        this.challenge = CHALLENGE_MIC_VIEW
      } else if (location.hash == "#canGoBack") {
        this.challenge = CHALLENGE_CAN_GO_BACK
      } else if (location.pathname == '/externalRedirect' && location.hash == "#cookies") {
        this.challenge = CHALLENGE_COOKIE
      }
    }
    
    this.nextChallenge()
  }
}
</script>