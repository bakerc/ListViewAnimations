ListViewAnimations ([Play Store Demo][1])
===========

ListViewAnimations is an Open Source Android library that allows developers to easily create ListViews with animations.
Feel free to use it all you want in your Android apps provided that you cite this project and include the license in your app.

ListViewAnimations uses the [NineOldAndroids][2] library to support devices <3.0.
It also uses Roman Nurik's BETA [SwipeDismissListViewTouchListener][5] to support swipe to dismiss.

Setup
-----
* In Eclipse, just import the library as an Android library project.
* Project > Clean to generate the binaries you need, like R.java, etc.
* Then, just add ListViewAnimations as a dependency to your existing project and you're good to go!

Or:

* Download the .jar file from the [Downloads page][4]
* Add the .jar to your project's `libs` folder.

Usage
-----
This library uses the [Decorator Pattern][3] to stack multiple `AnimationAdapter`s on each other:

* Implement your own `BaseAdapter`, or reuse an existing one.
* Stack multiple `BaseAdapterDecorator`s on each other, with your `BaseAdapter` as a base.
* Set the `ListView` to your last `AnimationAdapter`.
* Set your last `AnimationAdapter` to the `ListView`.

Example:
-----

	/* This example will stack two animations on top of eachother */
	@Override
	protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);

		MyListAdapter mAdapter = new MyListAdapter(this, getItems());
		SwingBottomInAnimationAdapter swingBottomInAnimationAdapter = new SwingBottomInAnimationAdapter(mAdapter);
		SwingRightInAnimationAdapter swingRightInAnimationAdapter = new SwingRightInAnimationAdapter(swingBottomInAnimationAdapter);
		
		// Or in short notation:
		swingRightInAnimationAdapter = 
			new SwingRightInAnimationAdapter(
				new SwingBottomInAnimationAdapter(
						new MyListAdapter(this, getItems())));
		
		// Assign the ListView to the AnimationAdapter and vice versa
		swingRightInAnimationAdapter.setListView(getListView());
		getListView().setAdapter(swingRightInAnimationAdapter);
	}
	
	private class MyListAdapter extends com.haarman.listviewanimations.ArrayAdapter<String> {

		private Context mContext;

		public MyListAdapter(Context context, ArrayList<String> items) {
			super(items);
			mContext = context;
		}

		@Override
		public View getView(int position, View convertView, ViewGroup parent) {
			TextView tv = (TextView) convertView;
			if (tv == null) {
				tv = (TextView) LayoutInflater.from(mContext).inflate(R.layout.list_row, parent, false);
			}
			tv.setText(getItem(position));
			return tv;
		}
	}

Custom AnimationAdapters
-----
Instead of using the ready-made adapters in the `.swinginadapters.prepared` package, you can also implement your own `AnimationAdapter`.
Implement one of the following classes:

* `ResourceAnimationAdapter`
* `SingleAnimationAdapter`
* `AnimationAdapter`

See the examples.

Note
-----
* Using the one of the `AnimationAdapter` classes will hide the dividers between your listitems.

Developed By
-----
* Niek Haarman

License
-----

	Copyright 2013 Niek Haarman

	Licensed under the Apache License, Version 2.0 (the "License");
	you may not use this file except in compliance with the License.
	You may obtain a copy of the License at

	http://www.apache.org/licenses/LICENSE-2.0

	Unless required by applicable law or agreed to in writing, software
	distributed under the License is distributed on an "AS IS" BASIS,
	WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
	See the License for the specific language governing permissions and
	limitations under the License.

 [1]: https://play.google.com/store/apps/details?id=com.haarman.listviewanimations
 [2]: http://nineoldandroids.com/
 [3]: http://en.wikipedia.org/wiki/Decorator_pattern
 [4]: https://bitbucket.org/nhaarman/listviewanimations/downloads
 [5]: https://gist.github.com/romannurik/2980593