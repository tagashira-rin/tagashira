<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>A-Frame VR アプリ</title>
    <meta name="description" content="360度画像とglbモデルを表示するVRアプリ">
    <script src="https://aframe.io/releases/1.4.0/aframe.min.js"></script>
    <script src="https://cdn.jsdelivr.net/gh/donmccurdy/aframe-extras@v6.1.1/dist/aframe-extras.min.js"></script>
    <style>
      body {
        margin: 0;
        padding: 0;
        overflow: hidden;
      }
      .a-enter-vr-button {
        position: fixed;
        bottom: 20px;
        right: 20px;
        z-index: 999;
      }
    </style>
    <script>
      // ナビゲーションアイコンのコンポーネント
      AFRAME.registerComponent('nav-icon', {
        schema: {
          target: {type: 'string'},
          thumbnail: {type: 'string'}
        },
        init: function() {
          const el = this.el;
          const data = this.data;

          // マウスオーバー時の拡大
          el.addEventListener('mouseenter', function() {
            el.setAttribute('scale', '1.2 1.2 1.2');
          });

          // マウスアウト時に元のサイズに戻す
          el.addEventListener('mouseleave', function() {
            el.setAttribute('scale', '1 1 1');
          });

          // クリック時に360度画像を切り替え
          el.addEventListener('click', function() {
            document.querySelector('#sky').setAttribute('src', '#' + data.target);
            console.log('360度画像を切り替えました: ' + data.target);
          });
        }
      });
    </script>
  </head>
  <body>
    <a-scene>
      <!-- アセットのプリロード -->
      <a-assets>
        <!-- 360度画像 -->
        <img id="pano1" src="R0010034.JPG" crossorigin="anonymous">
        <img id="pano2" src="R0010095.JPG" crossorigin="anonymous">
        <img id="pano3" src="R0010036.JPG" crossorigin="anonymous">

        <!-- サムネイル画像（アイキャッチ） -->
        <img id="thumb1" src="R0010034.JPG" crossorigin="anonymous">
        <img id="thumb2" src="R0010095.JPG" crossorigin="anonymous">
        <img id="thumb3" src="R0010036.JPG" crossorigin="anonymous">

        <!-- 3Dモデル -->
        <a-asset-item id="dancing-model" src="akaei_HouseDancing (1).glb"></a-asset-item>
      </a-assets>

      <!-- 360度画像の表示 -->
      <a-sky id="sky" src="#pano1" rotation="0 -90 0"></a-sky>

      <!-- glbモデルの表示 -->
      <a-entity
        id="animated-model"
        position="0 0 -3"
        scale="3 3 3"
        rotation="0 0 0"
        gltf-model="#dancing-model"
        animation-mixer="clip: *; loop: true; timeScale: 1">
      </a-entity>

      <!-- ナビゲーションアイコン -->
      <!-- 場所1へのナビゲーション -->
      <a-entity
        position="-2 1 -3"
        nav-icon="target: pano1; thumbnail: thumb1">
        <a-image src="#thumb1" width="0.5" height="0.5"></a-image>
      </a-entity>

      <!-- 場所2へのナビゲーション -->
      <a-entity
        position="0 1 -3"
        nav-icon="target: pano2; thumbnail: thumb2">
        <a-image src="#thumb2" width="0.5" height="0.5"></a-image>
      </a-entity>

      <!-- 場所3へのナビゲーション -->
      <a-entity
        position="2 1 -3"
        nav-icon="target: pano3; thumbnail: thumb3">
        <a-image src="#thumb3" width="0.5" height="0.5"></a-image>
      </a-entity>

      <!-- カメラとカーソル -->
      <a-entity camera look-controls position="0 1.6 0">
        <a-cursor id="cursor" color="white" fuse="true" fuse-timeout="1500"></a-cursor>
      </a-entity>

      <!-- 環境光 -->
      <a-light type="ambient" color="#BBB"></a-light>
      <a-light type="directional" color="#FFF" intensity="0.6" position="-0.5 1 1"></a-light>
    </a-scene>

    <script>
      // モデルが読み込まれたときのイベントリスナー
      document.querySelector('#animated-model').addEventListener('model-loaded', function() {
        console.log('モデルが読み込まれました');
        // モデルのアニメーションを制御するための追加コード
        const model = document.querySelector('#animated-model').object3D;
        console.log('モデル情報:', model);
      });

      // シーンが読み込まれたときのイベントリスナー
      document.querySelector('a-scene').addEventListener('loaded', function() {
        console.log('A-Frameシーンが読み込まれました');
      });
    </script>
  </body>
</html>



このプログラムを以下のように改良
・初期状態では、akaei_HouseDancing (1).glbは表示、akaei_Dying.glb は非表示。
・アイキャッチ画像をクリック、タップ、VRゴーグルのトリガーを引くと、
akaei_HouseDancing (1).glb 非表示となり、akaei_Dying.glb が4秒間表示される。
その後、次の360度画像に切り替わり、再度akaei_HouseDancing (1).glbが表示され、akaei_Dying.glb は非表示になる。
以下、クリックイベントのたびに、akaei_HouseDancing (1).glbと、akaei_Dying.glbの表示、非表示が切り替わる。
ただし、すべてindex.htmlの中に、記載すること。