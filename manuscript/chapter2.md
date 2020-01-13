# 開始設計 CRUD 的新增與刪除功能

現在要來先建立一個 Blazor 專案的 CRUD之新增與修改功能，不過，在這裡的新增功能將不會有記事紀錄輸入畫面，而是自動產生一筆隨機記事紀錄。

## 建立專案會用到的資料模型

- 滑鼠右擊專案節點
- 在彈出功能表點選 [加入] > [新增資料夾]
- 使用 `Models` 作為該新資料夾的名稱
- 滑鼠右擊 [Models] 資料夾節點
- 在彈出功能表點選 [加入] > [類別]
- 出現 [新增項目 - BlazorOverview] 對話窗
- 請在 [名稱] 欄位，輸入 `MyNote.cs`
- 最後，請點選 [新增] 按鈕
 
  ![建立 MyNote 資料模型類別](Images/BlazorQO993.png)

- 使用底下程式碼來替換到這個檔案內的所有內容
 
  ![設計 MyNote 類別成員](Images/BlazorQO992.png)

```csharp
namespace BlazorOverview.Models
{
    public class MyNote
    {
        public string Title { get; set; }
    }
}
```

## 建立與設計 Blazor 元件 - 顯示所有記事紀錄

- 滑鼠右擊 [Pages] 資料夾節點
- 在彈出功能表點選 [加入] > [新增項目]
- 出現 [新增項目 - BlazorOverview] 對話窗
- 請確認該對話窗左方的清單位於 [已安裝] > [Visual C#] > [ASP.NET Core] 節點上
- 在該對話窗的中間區域，點選 [Blazor 元件] 這個選項
- 請在 [名稱] 欄位，輸入 `MyNotes.razor`
- 最後，請點選 [新增] 按鈕
  
  ![建立一個 MyNotes 之 Blazor 元件](Images/BlazorQO991.png)

  I> ## 說明
  I>
  I> 每一個 Blazor 元件都可以成為具有路由的實際網頁 URL，不過，在這裡的練習過程中，將會簡化這些設計內容，將整個 CRUD 應用程式，濃縮在一個 Blazor Component 元件上，也讓讀者可以體會到 Blazor 的一個很重要的特色，那就是 Blazor 的元件是可以做到重複使用的目的。
  I> 
  
 - 使用底下 Razor 程式碼來替換到這個檔案內的所有內容
 
  ![MyNotes 元件的設計結果](Images/BlazorQO990.png)

```html
@using BlazorOverview.Models

<h3>我的記事</h3>

@*這裡是 HTML 的標記宣告*@
<table class="table">
    <thead>
        <tr>
            <th>事項</th>
            <th>修改</th>
            <th>刪除</th>
        </tr>
    </thead>
    <tbody>
        @*列出集合清單中的每一筆紀錄到 HTML Table 內*@
        @foreach (var NoteItem in Notes)
        {
            <tr>
                @*透過資料綁定，把集合清單內的紀錄屬性，顯示在網頁上*@
                <td>@NoteItem.Title</td>
                @*這個按鈕尚未進行任何設計*@
                <td><input type="button" class="btn btn-primary" value="修改" /></td>
                <td>
                    @*透過 Blazor 的資料綁定，將刪除按鈕的點選事件，綁定到 C# 的委派處理方法*@
                    <input type="button" class="btn btn-danger" value="刪除"
                           @onclick="()=>Delete(NoteItem)" />
                </td>
            </tr>
        }
    </tbody>
</table>
<div>
    @*透過 Blazor 的資料綁定，將新增按鈕的點選事件，*@
    @*綁定到 C# 的委派處理方法*@
    <input type="button" class="btn btn-primary" @onclick="Add" value="新增" />
</div>

@code {
    // 儲存要顯示的集合清單內的所有紀錄
    public List<MyNote> Notes { get; set; } = new List<MyNote>();

    // 元件建立的時候，所要執行的初始化工作
    protected override void OnInitialized()
    {
        // 預設建立的集合清單紀錄
        Notes = new List<MyNote>()
        {
            new MyNote { Title= "買蘋果" },
            new MyNote { Title="買西瓜" }
        };
    }
    // 新增按鈕的點選事件之處理委派方法
    private void Add()
    {
        //加入一筆紀錄到集合清單內
        Notes.Add(new MyNote { Title = $"新事項 {DateTime.Now.ToString()}" });
    }
    // 刪除按鈕的點選事件之處理委派方法
    private void Delete(MyNote note)
    {
        //從集合清單中刪除所選擇的紀錄
        Notes.Remove(note);
    }
}
```

  T> ## 提示
  T>
  T> 對於上述的程式碼與HTML標記，代表什麼意思，可以參考上面相關註解文字內容。
  T> 

  I> ## 說明
  I>
  I> 每個 Blazor 元件就是一個 Razor 元件檔案，附檔案名稱為 .razor，其是由 HTML 標記與 C# 程式語言所組成；在上面的 MyNotes.razor 檔案， `@code{...}` 區塊為要設計的 C# 程式語言，其他的部分則是 HTML 宣告標記語言，不過，可以使用 `@` 符號，讓 HTML 標記宣告語言參雜 C# 程式語言在其中。
  I> 
  
## 在 Blazor 專案首頁，加入此元件

- 在 [Pages] 資料夾中找到 [Index.razor] 這個檔案
- 打開這個檔案
- 使用底下 Razor 程式碼來替換到這個檔案內的所有內容

  T> ## 說明
  T>
  T> 想要在 Blazor 專案內的元件，使用該專案內的其他元件，可以把每個元件都是為一個 HTML 標籤，在這裡僅加入 `<MyNotes />` 這個元件參考，讓這個剛剛設計的新 Blazor 元件，可以顯示在首頁上
  T> 

```html
@page "/"

<h1>Hello, world!</h1>

Welcome to your new app.

@*使用 HTML Tag 標籤宣告方式，宣告這裡要顯示 MyNotes 這個 Blazor 元件*@
<MyNotes />
```

T> ## 提示
T>
T> 每個 Blazor 元件於設計完成之後，只要相對應的命名空間有宣告，便可以在 HTML 標記文件中，直接使用這個元件名稱作為一個 HTML 標記來使用，可謂相當的直覺與容易使用。
T> 

I> ## 說明
I>
I> 透過這樣的練習設計，簡化了每次執行專案的時候，就可以立即在首頁上看到這次設計的 CRUD 應用的元件了。
I> 

## 執行這個專案

- 請點選工具列上方的綠色三角形，或者按下 F5 ，開始執行這個 Blazor 專案
- 此時，將會在瀏覽器上出現底下畫面
  
  ![Blazor 專案具有新增與刪除執行結果](Images/BlazorQO989.png)

  I> ## 說明
  I>
  I> 這裡會預先看到兩筆記事紀錄，這是因為在該 MyNotes.razor 元件建立的時候，透過 `OnInitialized` 方法呼叫，就已經預先產生兩筆記事紀錄了，不過，由於這些記事紀錄都是使用電腦記憶體作為儲存之用，因此，每次重啟這個專案的時候，這些記事紀錄都會消失不見。
  I> 

- 點選 [新增] 按鈕兩次
- 此時將會自動加入兩筆紀錄到集合清單內
- 這裡是透過 Blazor 內建的資料綁定 Data Binding 機制，將會顯示在瀏覽器網頁上，如下面螢幕截圖
  
  ![新增兩筆紀錄](Images/BlazorQO988.png)

- 點選到數第二筆紀錄的 紅色 [刪除] 按鈕
- 此時，該筆紀錄將會從集合清單內刪除掉，並且即時更新在網頁上，如下面螢幕截圖
  
  ![刪除其中一筆紀錄的執行結果](Images/BlazorQO987.png)

## 結論

現在已經完成具有新增與刪除 Blazor 專案了，不過，所有集合清單資料，都是儲存在記憶體內，一旦專案關閉結束並且重新執行，這些集合清單資料將會消失不見，這個問題將會在稍後，使用資料庫的方式來解決。
