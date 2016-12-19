# 最火Android开源项目DynamicGridView使用
---
开源项目地址:[https://github.com/open-android/DynamicGridView](https://github.com/open-android/DynamicGridView)

# 运行效果
  ![](http://upload-images.jianshu.io/upload_images/4037105-df28bfd516527207.gif?imageMogr2/auto-orient/strip)

## 使用步骤
### 1. 在project的build.gradle添加如下代码(如下图)

	allprojects {
	    repositories {
	        ...
	        maven { url "https://jitpack.io" }
	    }
	}

![](http://oi5nqn6ce.bkt.clouddn.com/itheima/booster/code/jitpack.png)

### 2. 在Module的build.gradle添加依赖

    compile 'com.github.open-android:DynamicGridView:0.1.0'

### 3. 复制如下代码到xml

    <org.askerov.dynamicgrid.DynamicGridView
          android:id="@+id/dynamic_grid"
          android:layout_height="wrap_content"
          android:layout_width="match_parent"
          android:numColumns="@integer/column_count"      
    />


### 4. 复制如下代码到Activity


    gridView = (DynamicGridView) findViewById(R.id.dynamic_grid);
    // 第一个参数：上下文
    // 第二个参数：adapter展示需要用到的集合数据
    // 第三个参数：gridview一共有多少列
    gridView.setAdapter(new MyDynamicGridAdapter(this, itemsList, 3));
    gridView.setOnDragListener(new DynamicGridView.OnDragListener() {
            @Override
            public void onDragStarted(int position) {
                Log.d(TAG, "drag started at position " + position);
            }

            @Override
            public void onDragPositionsChanged(int oldPosition, int newPosition) {
                Log.d(TAG, String.format("drag item position changed from %d to %d", oldPosition, newPosition));
            }
        });
        gridView.setOnItemLongClickListener(new AdapterView.OnItemLongClickListener() {
            @Override
            public boolean onItemLongClick(AdapterView<?> parent, View view, int position, long id) {
                gridView.startEditMode(position);
                return true;
            }
        });

        gridView.setOnItemClickListener(new AdapterView.OnItemClickListener() {
            @Override
            public void onItemClick(AdapterView<?> parent, View view, int position, long id) {
                Toast.makeText(GridActivity.this, parent.getAdapter().getItem(position).toString(),
                        Toast.LENGTH_SHORT).show();
            }
        });
       
       @Override
       public void onBackPressed() {

	        if (gridView.isEditMode()) {
	            gridView.stopEditMode();
	        } else {
	            super.onBackPressed();
                //进行数据持久化操作
	        }
      }
> 细节注意：
> 
>当前开源项目只实现了拖拽特效,拖拽完成退出应用之后并没有实现数据持久化功能,如需要进行数据的持久化,需要监听返回按钮在else方法实现数据持久化操作。




### 5. 复制如下代码到Adapter

    public class CheeseDynamicAdapter extends BaseDynamicGridAdapter {
    public CheeseDynamicAdapter(Context context, List<?> items, int columnCount) {
        super(context, items, columnCount);
    }

    @Override
    public View getView(int position, View convertView, ViewGroup parent) {
        CheeseViewHolder holder;
        if (convertView == null) {
            convertView = LayoutInflater.from(getContext()).inflate(R.layout.item_grid, null);
            holder = new CheeseViewHolder(convertView);
            convertView.setTag(holder);
        } else {
            holder = (CheeseViewHolder) convertView.getTag();
        }
        holder.build(getItem(position).toString());
        return convertView;
    }

    private class CheeseViewHolder {
        private TextView titleText;
        private ImageView image;

        private CheeseViewHolder(View view) {
            titleText = (TextView) view.findViewById(R.id.item_title);
            image = (ImageView) view.findViewById(R.id.item_img);
        }

        void build(String title) {
            titleText.setText(title);
            image.setImageResource(R.mipmap.ic_launcher);
        }
    }
}

> 细节注意：
> 
> 必须继承BaseDynamicGridAdapter。

欢迎关注微信公众号

![](http://oi5nqn6ce.bkt.clouddn.com/itheima/booster/code/qrcode.png)
