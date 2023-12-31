<script setup>

    import { ref } from 'vue';
    import { useUserStore } from '../store/userStore';
    import PopUpBox from './PopUpBox.vue';

    const headerSprites = [
        {text: '試算優惠', path: '/'},
        {text: '卡片總覽', path: '/cards'},
        {text: '分類排行', path: '/ranking'},
        {text: '通知', path: '/notification'}
    ];

    const avatarHovering = ref(false);

    const userStore = useUserStore();
    const { mNo, userName, email } = userStore;
    
    // 登入
    const logining = ref({ on: false });
    const loginMNo = ref('');
    const loginPassword = ref('');

    const confirmPassword = (inputMNo, inputPassword) => {
        // 發請求給後端確認會員編號和密碼能不能對上
    };

    // 註冊
    const registerMode = ref({ on: false });
    const registerName = ref('');
    const registerPassword = ref('');
    const registerEmail = ref('');

    const register = (registerName, registerPassword, registerEmail) => {
        // 發請求給後端確認這密碼能不能行
    };

    // 設定
    const editingNickname = ref(false);
    const editingEmail = ref(false);

    const settingMode = ref({ on: false });

</script>

<template>
    <div class="header">
        <router-link to="/" class="logo">CCC</router-link>
        <ul>
            <li v-for="(item, index) in headerSprites">
                <router-link :to="item.path" class="aIcon" :style="{ backgroundPositionX: 16-index*42 + 'px' }">
                    {{ item.text }}
                </router-link>
            </li>
            
            <li v-if="mNo" @mouseenter="avatarHovering=true" @mouseleave="avatarHovering=false" class="liForProfile">
                <button class="profilePic roundBtn"></button>
                <ul v-if="avatarHovering" class="profileList">
                    <li class="myCollection"><router-link to="/cards">我的收藏</router-link></li>
                    <li @click="settingMode.on=true" class="settingSwitch">設定</li>
                    <hr>
                    <li class="logout">登出</li>
                </ul>
            </li>
            <li v-else>
                <button @click="logining.on=true" class="loginBtn roundBtn">登入</button>
            </li>

        </ul>
    </div>

    <PopUpBox :show="logining">
        <template #tab>登入</template>
        <template #content>
            <div class="inputInfo">

                <label>
                    會員編號：<br>
                    <input type="text" v-model="loginMNo">
                </label>
                <label>
                    密碼：<br>
                    <input type="password" v-model="loginPassword">
                </label>
                <button @click="confirmPassword(loginMNo, loginPassword)" type="button">確認</button>

                <hr>
                <span>還不是會員？</span>
                <button @click="registerMode.on=true; logining.on=false" type="button">點此註冊</button>

            </div>
        </template>
    </PopUpBox>

    <PopUpBox :show="registerMode">
        <template #tab>註冊</template>
        <template #content>
            <div class="inputInfo">
                <label>
                    暱稱：<br>
                    <input type="text" v-model="registerName" placeholder="請輸入您的暱稱">
                </label>
                <label>
                    密碼：<br>
                    <input type="password" v-model="registerPassword" placeholder="請設定您的密碼">
                </label>
                <label>
                    信箱：<br>
                    <input type="text" v-model="registerEmail" placeholder="請填入您的信箱">
                </label>
                <button @click="register(registerName, registerPassword, registerEmail)" type="button">
                    確認
                </button>
            </div>
        </template>
    </PopUpBox>

    <PopUpBox :show="settingMode">
        <template #tab>設定</template>
        <template #content>
            <ul class="settingUl">
                <li class="nickname">
                    暱稱：
                    <template v-if="editingNickname">
                        <input type="text">
                        <button @click="editingNickname=false" class="cancelBtn">取消</button>
                        <button @click="editingNickname=false">確認</button>
                    </template>
                    <template v-else><span class="userInfo">{{ userName }}</span>
                        <button @click="editingNickname=true" class="editBtn">編輯</button>
                    </template>
                </li>
                <li class="email">
                    信箱：
                    <template v-if="editingEmail">
                        <input type="text">
                        <button @click="editingEmail=false" class="cancelBtn">取消</button>
                        <button @click="editingEmail=false">確認</button>
                    </template>
                    <template v-else><span class="userInfo">{{ email }}</span>
                        <button @click="editingEmail=true" class="editBtn">編輯</button>
                    </template>
                </li>
                <li class="notificationSetting">
                    <router-link to="/cards" @click="settingMode.on=false">通知設定 &gt;</router-link>
                </li>
            </ul>
        </template>
    </PopUpBox>        

</template>

<style lang="less" scoped>

    .header {
        display: flex;
        align-items: center;
        justify-content: space-between;
        position: fixed;
        top: 0;
        z-index: 2;
        box-sizing: border-box;
        padding: 0 20px;
        width: 100vw;
        height: 80px;
        background-color: #81D9EC;
        

        .logo {
            width: 160px;
            height: 100%;
            background-image: '';
            /* 下面這段有logo之後刪掉 */
            color: #009DBF;
            font-size: 48px;
            text-align: center;
            text-decoration: none;
            line-height: 80px;
            &:hover {
                text-shadow: 0 0 6px white;
            }
        }

        ul {
            display: flex;
            align-items: center;
            justify-content: space-evenly;
            margin-right: 16px;
            width: 300px;
            li {
                display: flex;
                flex-direction: column;
                
                .aIcon {
                    display: inline-block;
                    width: 50px;
                    height: 50px;
                    text-decoration: none;
                    text-align: center;
                    line-height: 7.6;
                    font-size: 12px;
                    color: white;
                    background: url('../assets/images/headerSprites.svg') no-repeat left 0;
                    background-size: 300%;
                    transition: .2s;
                    &:hover {
                        color: #009DBF;
                        background-image: url('../assets/images/headerSpritesHover.svg');
                    }
                }
            }
        }

        li {
            position: relative;
            padding: 10px 0;
            .roundBtn {
                width: 50px;
                height: 50px;
                border: 1px solid gray;
                border-radius: 50%;
            }
            .profilePic {
                background: white url('../assets/images/預設頭像.svg') no-repeat center 8px;
                background-size: 60%;
            }
            .loginBtn {
                font-size: 16px;
                color: gray;
                background-color: white;
                transition: .2s;
                &:hover {
                    color: white;
                    background-color: #009DBF;
                }
            }
            .profileList {
                overflow: hidden;
                display: flex;
                flex-direction: column;
                position: absolute;
                top: 70px;
                right: -20px;
                padding: 4px 0;
                width: 140px;
                border: 1px solid gray;
                border-radius: 12px;
                background-color: white;
                a {
                    color: black;
                    text-decoration: none;
                    &:hover {
                        color: #009DBF;
                    }
                }
                li {
                    margin: 4px 0;
                    padding: 8px 0;
                    width: 100%;
                    text-indent: 50px;
                    background: url('../assets/images/profileSprites.svg') no-repeat 16px;
                    background-size: 20%;
                    cursor: pointer;
                    &:hover {
                        color: #009DBF;
                        background-image: url('../assets/images/profileSpritesHover.svg');
                    }
                }
                .myCollection {
                    background-position-y: 12px;
                }
                .settingSwitch {
                    background-position-y: -19px;
                }
                .logout {
                    background-position-y: -50px;
                }
                hr {
                    margin: 4px 0;
                    width: 100px;
                    border: 1px solid lightgray;
                }
            }
        }

    }

    .inputInfo {
        display: flex;
        flex-direction: column;
        align-items: center;
        margin-top: 10px;
        label {
            margin: 10px 40px;
            input {
                margin-top: 10px;
                text-indent: 2px;
            }
        }
        button {
            margin: 20px;
            padding: 0 10px;
            height: 40px;
            color: white;
            background-color: #81D9EC;
            border: 1px solid #009DBF;
            border-radius: 12px;
            &:hover {
                background-color: #009DBF;
            }
        }
        hr {
            margin: auto;
            margin-bottom: 20px;
            width: 90%;
            border: 1px solid lightgray;
        }
    }


    .settingUl {
        width: 380px;
        li {
            display: flex;
            align-items: center;
            position: relative;
            height: 60px;
            text-indent: 70px;
            background: url('../assets/images/settingSprites.svg') no-repeat 16px;
            background-size: 10%;
            .userInfo {
                text-indent: 0;
            }
            button {
                position: absolute;
                right: 16px;
                color: gray;
                border: none;
                background: none;
                &:hover {
                    color: #009DBF;
                }
            }
            .cancelBtn {
                right: 50px;
            }
            a {
                color: gray;
                text-decoration: none;
                &:hover {
                    color: #009DBF;
                }
            }
        }

        .nickname {
            background-position-y: 12px;
        }
        .email {
            background-position-y: -38px;
        }
        .notificationSetting {
            background-position-y: -88px;
        }
    }
    
</style>