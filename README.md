# inventapp

package com.example.manasi.inventory2;


        import android.support.v7.app.AppCompatActivity;
        import android.os.Bundle;

        import java.util.ArrayList;

        import android.app.ListActivity;
        import android.os.Bundle;
        import android.util.SparseBooleanArray;
        import android.view.View;
        import android.view.View.OnClickListener;
        import android.widget.ArrayAdapter;
        import android.widget.Button;
        import android.widget.EditText;

public class MainActivity extends ListActivity {

    /** Items entered by the user is stored in this ArrayList variable */
    ArrayList list = new ArrayList();

    /** Declaring an ArrayAdapter to set items to ListView */
    ArrayAdapter adapter;

    /** Called when the activity is first created. */
    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        /** Setting a custom layout for the list activity */
        setContentView(R.layout.activity_main);

        /** Reference to the add button of the layout main.xml */
        Button btn = (Button) findViewById(R.id.btnAdd);

        /** Reference to the delete button of the layout main.xml */
        Button btnDel = (Button) findViewById(R.id.btnDel);

        /** Defining the ArrayAdapter to set items to ListView */
        adapter = new ArrayAdapter(this, android.R.layout.simple_list_item_multiple_choice, list);

        /** Defining a click event listener for the button "Add" */
        OnClickListener listener = new OnClickListener() {
            @Override
            public void onClick(View v) {
                EditText edit = (EditText) findViewById(R.id.txtItem);
                if(edit.getText().toString().length()>0) {
                    list.add(edit.getText().toString());
                    edit.setText("");
                }
                adapter.notifyDataSetChanged();
            }
        };

        /** Defining a click event listener for the button "Delete" */
        OnClickListener listenerDel = new OnClickListener() {
            @Override
            public void onClick(View v) {
                /** Getting the checked items from the listview */
                SparseBooleanArray checkedItemPositions = getListView().getCheckedItemPositions();
                int itemCount = getListView().getCount();

                for(int i=itemCount-1; i >= 0; i--){
                    if(checkedItemPositions.get(i)){
                        adapter.remove(list.get(i));
                    }
                }
                checkedItemPositions.clear();
                adapter.notifyDataSetChanged();
            }
        };

        /** Setting the event listener for the add button */
        btn.setOnClickListener(listener);

        /** Setting the event listener for the delete button */
        btnDel.setOnClickListener(listenerDel);

        /** Setting the adapter to the ListView */
        setListAdapter(adapter);
    }
}

<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:orientation="vertical" >

    <EditText
        android:id="@+id/txtItem"
        android:layout_width="240dp"
        android:layout_height="wrap_content"
        android:inputType="text"
        android:hint="@string/hintTxtItem"
        />

    <Button
        android:id="@+id/btnAdd"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:text="@string/lblBtnAdd"
        android:layout_toRightOf="@id/txtItem"
        />

    <TextView
        android:id="@android:id/empty"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:layout_below="@id/txtItem"
        android:text="@string/txtEmpty"
        android:gravity="center_horizontal"
        />

    <ListView
        android:id="@android:id/list"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:layout_below="@id/txtItem"
        android:background="@color/colorAccent"
        android:choiceMode="multipleChoice"></ListView>

    <Button
        android:id="@+id/btnDel"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:layout_alignParentBottom="true"
        android:background="@android:drawable/bottom_bar"
        android:text="@string/lblBtnDel"
        android:onClick="deletePressed"/>
</RelativeLayout>
