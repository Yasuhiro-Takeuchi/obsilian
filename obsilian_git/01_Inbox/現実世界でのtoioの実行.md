前回はこちら[[シミュレータでのtoioの実行]]

#プログラミング言語/unity 
#プログラミング言語/toio 

参考にしたサイト
[キューブの操作](https://note.com/npaka/n/n65434b469d2d)

[[シミュレータでのtoioの実行]]ではtoioを回転させた。
今回はキューブを回転させるコードを、キューブのキー操作とXY座標表示を行うコードに変更する。


1. Hierarchyウィンドウで「RotateCube」を削除。
2. Hierarchyウィンドウの「＋ → UI → Text」で「Text」を追加し、「Label」という名前を指定。
   白文字で左上に配置して、初期値「(0, 0)」にしています。
3. Hierarchyウィンドウで、「＋ → Create → Empty Object」で空のオブジェクトを生成し、「ControlCube」という名前を指定。
4. Hierarchyウィンドウで「ControlCube」を選択し、Inspectorウィンドウで「ControlCube」を追加し、以下のように編集。
```
using UnityEngine;
using UnityEngine.UI;
using toio;

// キューブの操作
public class ControlCube : MonoBehaviour
{
    public ConnectType connectType; // 接続種別
    public Text label; // ラベル

    CubeManager cm; // キューブマネージャ

    // スタート時に呼ばれる
    async void Start()
    {
        // キューブの接続
        cm = new CubeManager(connectType);
        await cm.MultiConnect(1);
    }

    // フレーム毎に呼ばれる
    void Update()
    {
        // キューブのキー操作
        foreach (var cube in cm.syncCubes)
        {
            if (Input.GetKey(KeyCode.LeftArrow)) {
                cube.Move(-20, 20, 50);
            } else if (Input.GetKey(KeyCode.RightArrow)) {
                cube.Move(20, -20, 50);
            } else if (Input.GetKey(KeyCode.UpArrow)) {
                cube.Move(50, 50, 50);
            } else if (Input.GetKey(KeyCode.DownArrow)) {
                cube.Move(-50, -50, 50);
            }
        }

        // キューブのXY座標表示
        string text = "";
        foreach (var cube in cm.syncCubes)
        {
            text += "(" + cube.x + "," + cube.y + ")\n";
        }
        if (text != "") this.label.text = text;
    }
}
```
5. Hierarchyウィンドウで「ControlCube」を選択し、Hierarchyウィンドウの「Label」を「ControlCube」の「Label」にドラッグ&ドロップ。