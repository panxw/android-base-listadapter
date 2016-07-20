# android-base-listadapter
Android base list adapter more easily use.

## 1.BaseListAdapter
```
public abstract class BaseListAdapter<T, H> extends
		BaseAdapter {
	
	private List<T> mList;
	private Context mContext;

	public BaseListAdapter(Context context, List<T> list) {
		this.mContext = context;
		this.mList = list;
	}

	@Override
	public int getCount() {
		return this.mList != null ? this.mList.size() : 0;
	}

	@Override
	public T getItem(int position) {
		return this.mList.get(position);
	}

	@Override
	public long getItemId(int position) {
		return position;
	}

	@SuppressWarnings("unchecked")
	@Override
	public View getView(int position, View convertView, ViewGroup parent) {
		H viewHolder = null;

		if (convertView == null) {
			View view = onCreateItemView(LayoutInflater.from(mContext), parent);

			viewHolder = onCreateViewHolder();
			onViewHolderCreated(viewHolder, view);

			view.setTag(viewHolder);
			convertView = view;
		} else {
			viewHolder = (H) convertView.getTag();
		}

		T data = getItem(position);
		if (data != null) {
			onBindViewHolder(viewHolder, data);
		}

		return convertView;
	}

	protected abstract View onCreateItemView(LayoutInflater from, ViewGroup parent);
	
	protected abstract H onCreateViewHolder();
	
	protected abstract void onViewHolderCreated(H viewHolder, View view);

	protected abstract void onBindViewHolder(H viewHolder, T data);

}
```

## 2.extends BaseListAdapter and fill method
```
	private class ChannelListAdapter extends
			BaseListAdapter<Item, ChannelListViewHolder> {

		public ChannelListAdapter(Context context, List<Item> list) {
			super(context, list);
		}

		@Override
		protected View onCreateItemView(LayoutInflater inflater,
				ViewGroup parent) {
			return inflater.inflate(R.layout.live_channel_list_item_layout,
					parent, false);
		}
		
		@Override
		protected ChannelListViewHolder onCreateViewHolder() {
			return new ChannelListViewHolder();
		}
		
		@Override
		protected void onViewHolderCreated(ChannelListViewHolder viewHolder,
				View view) {
			viewHolder.icon = (ImageView) view.findViewById(R.id.icon);
			viewHolder.image = (ImageView) view.findViewById(R.id.image);
			viewHolder.title = (TextView) view.findViewById(R.id.title);
			viewHolder.nowPlay = (TextView) view.findViewById(R.id.nowPlay);
			viewHolder.nextPlay = (TextView) view.findViewById(R.id.nextPlay);
		}

		@Override
		protected void onBindViewHolder(ChannelListViewHolder viewHolder,
				Item data) {
			ImageLoaderUtil.displayNetImage(data.icon, viewHolder.icon);
			ImageLoaderUtil.displayNetImage(data.pic, viewHolder.image);

			viewHolder.title.setText(data.name);
			viewHolder.nowPlay.setText(data.title);
			viewHolder.nextPlay.setText(data.title2);
		}

	}

	private static class ChannelListViewHolder {
		public TextView nextPlay;
		public TextView nowPlay;
		public TextView title;
		public ImageView image;
		public ImageView icon;
	}

```
