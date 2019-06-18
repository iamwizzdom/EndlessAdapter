# EndlessAdapter
A powerful yet simple adapter for endless scrolling with Android AdapterViews in a few lines of code.




This release is all about data loading convenience for developers. It returns all the power and time to the developer who wishes to quickly implement endless loading of data from a feed or other data source. It also simplifies the whole process of guessing when the source has no more data and gives a consistent pattern for showing or hiding a load-more button or other widget.

It makes scrolling so easy in a few lines of code using clever techniques and clear object oriented syntax.

## How does it work?

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



