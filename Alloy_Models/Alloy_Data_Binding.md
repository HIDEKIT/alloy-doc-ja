#Alloy Data Binding

Introduction
Alloy Model-View Binding
Backbone Binding

##Introduction
コレクションのデータを変更した時、あなたは同時にビューを変更させたいだろう。
このコンセプトはデータバインディングとして知られています。
AlloyとBackboneはコレクションのデータとビューをバインド（紐付ける）するメカニズムを提供します。

When data in the collection changes, you may want to update the view simultaneously to keep information synchronized. This concept is known as data binding. Both Alloy and Backbone provide some mechanisms to bind collection data to a view.

##Alloy Model-View Binding
Allyにおいて、コレクションデータはViewオブジェクトと同期させられます。
きっとシングルモデルはビューコンポーネントと同期するだろう？＊
次のTitnaiumビューオブジェクトはコレクションのバインドをサポートします。

In Alloy, collection data can be synchronized to a view object, or a single model can be bound to a view component. The following Titanium view objects support binding to a Collection:

Since Alloy 1.0.0: TableView and View
Since Alloy 1.1.0: ButtonBar, CoverFlowView, ScrollableView, ToolBar and TabbedBar
Since Alloy 1.2.0: ListView

The Backbone add, change, destroy, fetch, remove, and reset events are monitored to update the data in the view.
Backboneのadd、change、destroy、fetch、remove、resetイベントは監視されています。
ビューのデータを変更するために。
To enable model-view binding, create a singleton or instance of a collection using the Collection tag in the XML markup of the main view, then add your TableView, View, ScrollableView, CoverFlowView, etc. object, specifying a few additional attributes in the markup, which are only specific to collection data binding. For ListView, add these attributes to the child ListSection tag; for ButtonBar and TabbedBar, add these attributes to the child Labels tag, and for ToolBar, add these attributes to the child Items tag.
モデルービューバインディングを利用するためには、シングルトンまたはコレクションのインスタンスをメインのビューのXMLマークアップタグを、
そして、ableView, View, ScrollableView, CoverFlowViewなどのお武家うとを追加します
明示します、２，３の追加属性をそのマークアップに追加します
それらはコレクションデータバインディングの特徴です。
ListViewの場合は、子のListSectionタグにそれらのタグを追加します。
 ButtonBar and TabbedBarの場合は、配下のLabelsタグに追加します。
ToolBarは配下のItemタグに追加します。


- dataCollection: specifies the collection singleton or instance to bind to the table. This is the name of the model file for singletons or the ID prefixed with the controller symbol ('$') for instances.
　データコレクション：

これはモデルファイル　シングルトンまたはIDのコントローラーシンボル$＊

- dataTransform: specifies an optional callback to use to format model attributes. The passed argument is a model and the return value is a modified model as a JSON object.
　
受け渡す引数はモデル、そして返り値は変更されたモデルをJSONオブジェクトとして変えいます。

- dataFilter: specifies an optional callback to use to filter data in the collection. The passed argument is a collection and the return value is an array of models.
　dataFilter: コレクションのデータを抽出する、オプションのコールバックを指定します。
引数はコレクション、そして返り値はモデルの配列です。

The only mandatory attribute is dataCollection, which specifies the collection singleton or instance to render.

必須属性はデータコレクションのみです。これにはコレクションシングルトンか描画をするインスタンスを指定します。

Create a repeater object, such as a TableViewRow, Label or ImageView, and place it inline with the TableView, View, ScrollableView, or CoverFlowView tag, or place it in a separate view and use the Require tag inline with the TableView, View, ScrollableView, or CoverFlowView tag to import it. The following view objects have restrictions for creating repeater objects:
リピーターオブジェクト、たとえば〜
次のビューオブジェクトは
リピーターオブジェクトを作るために制約があります。

- For ListView, the repeater object can only be a ListItem that is placed inline with the ListSection tag.
　ListViewはリピーターオブジェクトはインラインで配置されたListSectionタグだけです。

- For ButtonBar and TabbedBar, the repeater object can only be a Label that is placed inline with the Labels tag.
- For ToolBar, the repeater object can only be an Item that is placed inline with the Items tag.

To map model attributes, encase the attribute between curly brackets or braces ('{' and '}'). For example, to assign the Label.text property to the model's title attribute, use this notation: <Label text="{title}" />. For more complex transformations, such as using two model attributes, use the dataTransform callback to create a custom attribute.

モデルの属性をマップするため、中括弧とまたはブレイスで属性名を囲みます。
例えば、Label.textプロパティをモデルのtitle属性に割り当てるには、 <Label text="{title}" />と記述します。例えばより複雑な変換は、２つのモデルの属性や、属性をカスタマイズするためにdataTransformコールバックをを使用するような場合です。

In the controller code of the repeater object, you can use the special variable $model to reference the current model being iterated over. For example, to get the title attribute of the current model, use $model.title to access it.

コントローラーのリピーターオブジェクトのコードにおいて、反復される現在のモデルを参照するために特別変数$モデルを使用することができます。
例えば、現在のモデルのtitle属性を取得するため、$model.titleを使用してアクセするできます。

To bind a single model to a component, create a singleton or instance of a model using the Model tag in the XML markup of the main view and map the model attribute to the view component. Prefix the model name or id to the attribute when mapping the attribute to the view component. For example, using the book model example, to map the title attribute, use this notation: <Label text="{book.title}" />.

IMPORTANT: When using Alloy's model-view binding in a view-controller, you MUST call the $.destroy() function when closing a controller to prevent potential memory leaks. The destroy function unbinds the callbacks created by Alloy when the model-view syntax is used. For example, the code below calls the destroy function when the Window's close event is triggered.
重要:Alloy モデルビューバインディングを使用するとき、必ず$.destroy()関数を呼び出して下さい。これは
潜在的なメモリーリークを回避します。
destroyファンクションは、バインドを外す

例えば、以下のコードは


```
$.win.addEventListener("close", function(){
    $.destroy();
} 
```

The example below demonstrates how to display all book models in the collection by the 
author Mark Twain:


app/views/index.xml
```
<Alloy>
    <Collection src="book" />
    <Window class="container">
        <TableView dataCollection="book" dataTransform="transformFunction" dataFilter="filterFunction">
            <!-- Also can use Require -->
            <TableViewRow title="{title}" />
        </TableView>
    </Window>
</Alloy>
app/controllers/index.js
$.index.open();
 
// Encase the title attribute in square brackets
function transformFunction(model) {
    // Need to convert the model to a JSON object
    var transform = model.toJSON();
    transform.title = '[' + transform.title + ']';
    // Example of creating a custom attribute, reference in the view using {custom}
    transform.custom = transform.title + " by " + transform.author;
    return transform;
}

// Show only book models by Mark Twain
function filterFunction(collection) {
    return collection.where({author:'Mark Twain'});
}

// Trigger the synchronization
var library = Alloy.Collections.book;
library.fetch();
 
// Free model-view data binding resources when this view-controller closes
$.index.addEventListener('close', function() {
    $.destroy();
});
As the collection is updated, the view reflects the changes made to the models. If you want to suppress an update, specify {silent: true} in the options parameters when calling Backbone methods to change model data.

##Backbone Binding

The application can monitor Backbone events to trigger updates to the view.
アプリケーションはBackboneイベント　ビューの更新するトリガーとなるイベントを
監視しています

For instance, the code below demonstrates how to update a table, when a model object is added to a collection by monitoring the add event, which is triggered after a call to Backbone.Collection.add:
例えば、下のコードは例です、モデルオブジェクトにコレクションが追加された時、
Backbone.Collection.addが呼ばれた後に起動する
addイベントの監視によってテーブルを更新する方法の例です。

```
library.on('add', function(e){
    // custom function to update the content on the view
    updateFooView(library);
});
```

Another method is to selectively monitor changes. For instance, the code below demonstrates how to update data if a title changes in the collection:

別の方法は
例えば、
コレクションのtitleが変更されたが変更された場合に、データを更新する方法です。

```
library.on('change:title', function(e){
    // custom function to update the content on the view
    updateFooView(library);
});
```

This only works if the Backbone method fires the change event and does not enable {silent: true} as an option.
これはBackboneメソッドがchangeイベントを発火し、そして[silent:true]オプションが利用可能になっていいない場合、のみ、動作します。

