<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8">
  <title>メール認証付き掲示板ログイン</title>
</head>
<body>
  <h2>ログイン / 新規登録</h2>

  <input id="email" type="email" placeholder="メールアドレス"><br>
  <input id="password" type="password" placeholder="パスワード"><br><br>

  <button id="loginBtn">ログイン</button>
  <button id="signupBtn">新規登録</button>

  <p id="msg"></p>

  <script type="module">
    import { initializeApp } from "https://www.gstatic.com/firebasejs/10.12.1/firebase-app.js";
    import { getAuth, signInWithEmailAndPassword, createUserWithEmailAndPassword, sendEmailVerification, onAuthStateChanged } 
      from "https://www.gstatic.com/firebasejs/10.12.1/firebase-auth.js";

    const firebaseConfig = {
      apiKey: "AIzaSyDKQCGZj2bc7CQXTis773gs9juBwnVor0E",
      authDomain: "ich2414-a429b.firebaseapp.com",
      projectId: "ich2414-a429b",
      storageBucket: "ich2414-a429b.firebasestorage.app",
      messagingSenderId: "753800389456",
      appId: "1:753800389456:web:174ed1eb68df6dff8b4255"
    };

    const app = initializeApp(firebaseConfig);
    const auth = getAuth(app);

    const msg = document.getElementById("msg");

    // 新規登録
    document.getElementById("signupBtn").onclick = async () => {
      const email = document.getElementById("email").value;
      const password = document.getElementById("password").value;

      try {
        const userCredential = await createUserWithEmailAndPassword(auth, email, password);
        await sendEmailVerification(userCredential.user); // メール送信
        msg.textContent = "登録完了！確認メールを送信しました。メールを確認してからログインしてください。";
      } catch (e) {
        msg.textContent = "登録失敗：" + e.message;
      }
    };

    // ログイン
    document.getElementById("loginBtn").onclick = async () => {
      const email = document.getElementById("email").value;
      const password = document.getElementById("password").value;

      try {
        const userCredential = await signInWithEmailAndPassword(auth, email, password);

        if (userCredential.user.emailVerified) {
          msg.textContent = "ログイン成功！";
          location.href = "index.html"; // 掲示板へ遷移
        } else {
          msg.textContent = "メール確認が必要です。送信されたメールを確認してください。";
          await sendEmailVerification(userCredential.user); // 再送も可能
        }
      } catch (e) {
        msg.textContent = "ログイン失敗：" + e.message;
      }
    };
  </script>
</body>
</html># keijiban2
