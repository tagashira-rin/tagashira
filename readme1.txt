<!-- 
以下のコードをCodyに張り付けて index.html生成
機能
１．akaei_HouseDancing表示
２．アイキャッチ画像（正方形）をクリックすると、360度画像が切り替わる（R0010034.JPGとR0010095.JPGとR0010036.JPG）
３．<a-assets> タグを使用して画像やモデルをプリロードし、パフォーマンス向上とリソース管理の改善すること。
４．カーソル改良:VRモードでの使いやすさのために、fuseタイマーを追加しました（VRゴーグルでの視線による選択）。
 -->

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
  </head>
  <body>
    <a-scene>
      <!-- 360度画像の読み込み -->
      <a-sky src="R0010034.JPG" rotation="0 -90 0"></a-sky>

      <!-- glbモデルの表示 -->
      <a-entity
        id="animated-model"
        position="0 0 -3"
        scale="3 3 3"
        rotation="0 0 0"
        gltf-model="akaei_HouseDancing (1).glb"
        animation-mixer="clip: *; loop: true; timeScale: 1">
      </a-entity>

      <!-- カメラとカーソル -->
      <a-entity camera look-controls position="0 1.6 0">
        <a-cursor id="cursor" color="white"></a-cursor>
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


このプログラムを改良して、
Googleストリートビューのように、アイコンをマウスをクリック　または　スマホタップ 　または　VRゴーグルのトリガーを引くと、
360度画像が切り替わる（R0010034.JPGとR0010095.JPGとR0010036.JPGが順番に入れ替わる）ように変更して。

使用するアイコンは、
移動先の360度画像のアイキャッチ画像（正方形）を利用すること。カーソルを合わせると、アイキャッチ画像が120% 大きく表示される。

<a-assets> タグを使用して画像やモデルをプリロードし、パフォーマンス向上とリソース管理の改善すること。

ただし、すべてindex.htmlの中に、記載すること。