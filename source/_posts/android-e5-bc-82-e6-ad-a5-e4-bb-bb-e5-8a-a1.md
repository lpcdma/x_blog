title: "android异步任务"
date: 2014-03-31 02:28:54
tags:
id: 606
comment: false
categories:
  - 编程学习
---

<pre class="brush:java">	public static class PlaceholderFragment extends Fragment {

		private TextView textview;
		private Button button;
		private String image_path = "http://pic.nipic.com/2007-12-17/20071217222426246_2.jpg";
		private ImageView imageview;
		private ProgressDialog dialog;

		public PlaceholderFragment() {
		}

		public class MyTask extends AsyncTask&lt;String, Integer, Bitmap&gt; {

			// 任务执行之前操作
			@Override
			protected void onPreExecute() {
				// TODO Auto-generated method stub
				dialog = new ProgressDialog(getActivity());
				dialog.setTitle("提示");
				dialog.setMessage("下载中。。。。。");
				dialog.setProgressStyle(ProgressDialog.STYLE_HORIZONTAL);
				dialog.setCancelable(false);

				super.onPreExecute();
				dialog.show();
			}

			@Override
			protected void onProgressUpdate(Integer... values) {
				// TODO Auto-generated method stub
				super.onProgressUpdate(values);
				dialog.setProgress(values[0]);
			}

			// 完成耗时操作

			@Override
			protected Bitmap doInBackground(String... params) {
				// TODO Auto-generated method stub

				HttpClient httpClient = new DefaultHttpClient();
				HttpGet httpGet = new HttpGet(params[0]);
				Bitmap bitmap = null;
				ByteArrayOutputStream outputStream = new ByteArrayOutputStream();
				InputStream inputStream = null;
				try {
					HttpResponse httpResponse = httpClient.execute(httpGet);
					if (httpResponse.getStatusLine().getStatusCode() == 200) {
						inputStream = httpResponse.getEntity().getContent();
						long file_length = httpResponse.getEntity()
								.getContentLength();
						int len = 0;
						byte[] data = new byte[1024];
						int total_length = 0;
						while ((len = inputStream.read(data)) != -1) {

							total_length += len;
							int value = (int) ((total_length / (float) file_length) * 100);
							publishProgress(value);
							outputStream.write(data, 0, len);
						}
						/*
						 * HttpEntity httpEntity = httpResponse.getEntity();
						 * byte[] data = EntityUtils.toByteArray(httpEntity);
						 * bitmap = BitmapFactory.decodeByteArray(data, 0,
						 * data.length);
						 */
					}
					byte[] result = outputStream.toByteArray();
					bitmap = BitmapFactory.decodeByteArray(result, 0,
							result.length);
				} catch (ClientProtocolException e) {
					// TODO Auto-generated catch block
					e.printStackTrace();
				} catch (IOException e) {
					// TODO Auto-generated catch block
					e.printStackTrace();
				} finally {
					if (inputStream != null) {
						try {
							inputStream.close();
						} catch (IOException e) {
							// TODO Auto-generated catch block
							e.printStackTrace();
						}

					}
				}
				return bitmap;
			}

			//
			@Override
			protected void onPostExecute(Bitmap result) {
				// TODO Auto-generated method stub
				super.onPostExecute(result);
				dialog.dismiss();
				imageview.setImageBitmap(result);
			}

		}

		@Override
		public View onCreateView(LayoutInflater inflater, ViewGroup container,
				Bundle savedInstanceState) {
			View rootView = inflater.inflate(R.layout.fragment_main, container,
					false);
			textview = (TextView) rootView.findViewById(R.id.textView1);
			button = (Button) rootView.findViewById(R.id.button1);
			imageview = (ImageView) rootView.findViewById(R.id.imageView1);

			button.setOnClickListener(new View.OnClickListener() {

				@Override
				public void onClick(View v) {
					// TODO Auto-generated method stub
					/*
					 * new Thread(new Runnable() {
					 * 
					 * @Override public void run() { // TODO Auto-generated
					 * method stub HttpClient httpClient = new
					 * DefaultHttpClient(); HttpGet httpGet = new
					 * HttpGet(image_path); try { httpClient.execute(httpGet); }
					 * catch (ClientProtocolException e) { // TODO
					 * Auto-generated catch block e.printStackTrace(); } catch
					 * (IOException e) { // TODO Auto-generated catch block
					 * e.printStackTrace(); } } });
					 */

					new MyTask().execute(image_path);
					textview.setText("AAAAAA");

				}
			});
			return rootView;
		}

	}</pre>