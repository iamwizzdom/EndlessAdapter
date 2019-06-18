# EndlessAdapter
A powerful yet simple adapter for endless scrolling with Android AdapterViews in a few lines of code.




This release is all about data loading convenience for developers. It returns all the power and time to the developer who wishes to quickly implement endless loading of data from a feed or other data source. It also simplifies the whole process of guessing when the source has no more data and gives a consistent pattern for showing or hiding a load-more button or other widget.

It makes scrolling so easy in a few lines of code using clever techniques and clear object oriented syntax.

## How does it work?

In this library, the endless loading is handled at the level of the Adapter instead of the AdapterView.

All you need is to extend the EndlessAdapter and your ```getView``` method MUST return a call to super.getView.

It comes with a ```loadMoreItems``` method. This is the method that you MUST use to add items to this Adapter.

The basic functionality here is coded in the ```EndlessAdapter``` class which is a adapter for generic objects. This allows you to use the adapter for all kinds of data...e.g

```Java
class Boy{

private String name;
private double height;
private int age;
//Other code

}
```

Then to create an ```EndlessAdapter``` for boys:

```Java
public class BoysAdapter extends EndlessAdapter<Boy>{




        ViewHolder holder = null;


        if (convertView == null) {
            LayoutInflater inflater = LayoutInflater.from(parent
                    .getContext());

            holder = new ViewHolder();

            convertView = inflater.inflate(
                    R.layout.list_cell, parent, false);


            holder.nameView = convertView.findViewById(R.id.cell);

            // minimize the default image.
            convertView.setTag(holder);
        } else {
            holder = (ViewHolder) convertView.getTag();
        }

        Boy boy = getItem(position);

        try {
            holder.nameView.setText(boy.getName());
            
            ///Other data rendering codes.

        } catch (Exception e) {
            e.printStackTrace();
        }

        return super.getView(position,convertView,parent);

}
```

One thing you must ensure in your own implementation of the adapter e.g in ```BoysAdapter``` is that in your getView method, the last line of code must be a call that returns ```super.getView(position,convertView,parent)``` just as in the above ```getView``` method.


The adapter detects when the user scrolls to the bottom of the page and calls an ```onScrollToBottom``` method.
When ```onScrollToBottom``` is fired, the user's implementation must ask the server for 
more data, provided the ```moreItemsCouldBeAvailable``` parameter of the method is true. When this data comes, the user must pass it(after decoding it) into a List of items, which it then passes to the ```loadMoreItems``` method of the adapter.

With the ```moreItemsCouldBeAvailable``` variable, the adapter is able to guess if the server might have more content and so it advises the user accordingly to either call for more items or not or not in the ```onScrollToBottom``` 

When the ```loadMoreItems``` has finished executing, the ```onFinishedLoading``` method is called.

When the user scrolls away from the bottom,  the ```onScrollAwayFromBottom``` is called.


```Java

   adapter = new ModelAdapter() {
            @Override
            public void onScrollToBottom(int bottomIndex, boolean moreItemsCouldBeAvailable) {
            
                if (moreItemsCouldBeAvailable) { 
                    makeYourServerCallForMoreItems();
                } else {
                    if (loadMore.getVisibility() != View.VISIBLE) {
                        loadMore.setVisibility(View.VISIBLE);
                    }
                }
            }

            @Override
            public void onScrollAwayFromBottom(int currentIndex) { 
                loadMore.setVisibility(View.GONE);
            }

            @Override
            public void onFinishedLoading(boolean moreItemsReceived) { 
                if (!moreItemsReceived) {
                    loadMore.setVisibility(View.VISIBLE);
                }
            }
        };

```

When the server has no more items you may wish to enable a load-more button, the example above also shows how to do that. The button is the ```loadMore``` variable in the above example. 


For a complete example of the adapter in action, see:
https://github.com/gbenroscience/EndlessAdapter/tree/master/app/src/main/java/com/itis/libs/examples.

The example includes a Mock server which serves as a local source of data to the adapter.
Calls from the client to the mock server causes the server to respond with a json array of names of great men/women; both past and present. The server is designed to at times mimic a no data situation. 




You can see the adapter in operation at:

https://youtu.be/E-Mi-GUu6Ks

![alt-text](https://github.com/gbenroscience/EndlessAdapter/blob/master/gitassets/ssc.gif)

## Adding EndlessAdapter to your project:
 

Add it in your root build.gradle at the end of repositories:

```
	allprojects {
		repositories {
			...
			maven { url 'https://jitpack.io' }
		}
	}
```


Also add to your app's build.gradle file, under your dependencies, add:
```
dependencies {
	        implementation 'com.github.gbenroscience:EndlessAdapter:0.1.1'
	}
```

Thanks!









