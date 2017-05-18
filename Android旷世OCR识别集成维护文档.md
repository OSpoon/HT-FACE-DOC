## 旷世OCR识别集成维护文档
##### 1.后台地址 : [https://faceid.com/](https://faceid.com/ "后台地址")
##### 2.后台介绍
###### 2.1:应用配置
![](http://i.imgur.com/bG7TmSz.png)
	
	注:1.绑定因的Appkey请联系FaceID商务
	   2.当前Applkey的类型
	   3.当前Appkey的使用量

![](http://i.imgur.com/mMzfLZY.png)

	注:当前AppKey及AppSecret,调用旷世相关接口时需要

![](http://i.imgur.com/2llNXqz.png)

	注:1.新应用接入时,需到旷世后台进行BundleID绑定,通过审核后即可使用
	   2.此绑定有有效期限制,点击相应的查看可申请有效期延期操作,及SDK权限管理

###### 2.2:文档介绍 [https://faceid.com/pages/documents](https://faceid.com/pages/documents "接口文档")

###### OCRIDCARD API
		1.描述:检测和识别中华人民共和国第二代身份证。

		2.调用URL：https://api.faceid.com/faceid/v1/ocridcard
		
		注意：在生产环境中，请使用HTTPS的通信方式。HTTP方式的通信属于不安全链路，存在安全风险，请勿在生产环境中使用。在生产环境中使用HTTP方式的，将无法得到服务可靠性保障。

		调用方法：
		
		请求类型:POST
		
		请求参数(详细请参照官网):api_key/api_secret/image/legality
		
		响应参数(详细请参照官网):request_id/time_used/address/birthday/gender/id_card_number/name/race/side/issued_by/valid_date/head_rect/legality

######  返回示例:
		
	{
	    "time_used": 320,
	    "request_id": "1457432550,b70ab3a8-ee37-4f90-a2bd-007e23a970e2",
	    "address": "北京市海淀区XXXX",
	    "birthday": {
	        "day": "2",
	        "month": "6",
	        "year": "1991"
	    },
	    "head_rect": {
	        "lt": {
	            "x": 0.619915,
	            "y": 0.19362
	        },
	        "rt": {
	            "x": 0.921812,
	            "y": 0.198438
	        },
	        "rb": {
	            "x": 0.927187,
	            "y": 0.737717
	        },
	        "lb": {
	            "x": 0.621641,
	            "y": 0.735261
	        }
	    },
	    "gender": "男",
	    "id_card_number": "xxxxxx19910602xxxx",
	    "name": "陈XX",
	    "race": "汉",
	    "side": "front",
	    "legality": {
	        "ID Photo": 0.855,
	        "Temporary ID Photo ": 0,
	        "Photocopy": 0.049,
	        "Screen": 0.096,
	        "Edited": 0
	    }
	}

###### 调用示例

 	curl "https://api.faceid.com/faceid/v1/ocridcard" –F api_key=<api_key> -F api_secret=<api_secret> –F image=@id.jpg

##### SDK及外围代码集成修改
###### MegIDCardQuality身份证识别所提供Demo
![](http://i.imgur.com/gZ3bS5T.png)
###### MegLive人脸识别所提供Demo
![](http://i.imgur.com/mvizAcu.png)
###### BankCard银行卡识别所提供Demo
![](http://i.imgur.com/hXIEIN3.png)

###### android_n22_ocr工程整合
	基于content-onlineauthsdktest-MegLive-MegIDCardQuality_android_20170315_24集成的人脸识别和身份证识别
	基于bankcard_android_sdk_20170701_华泰保险_v1.0.0集成的银行卡(此版本问测试版本sdk)

###### 身份证识别部分:
	1.MegviiIDCardQuality_1.2.0.jar
	2.libMegviiIDCardQuality_1.2.0.so
	3.idcardmodel
	4.megviiidcard_0_2_0_model

###### 人脸识别部分:
	1.licensemanager-v1.1.jar
	2.livenessdetection-proguard-2.4.4.jar
	3.liblivenessdetection_v2.4.4.so
	4.livenessmodel
	5.meglive_eye_blink.mp3
	6.meglive_failed.mp3
	7.meglive_mouth_open.mp3
	8.meglive_pitch_down.mp3
	9.meglive_success.mp3
	10.meglive_well_done.mp3
	11.meglive_yaw.mp3
	12.model

###### 银行卡识别部分:
	1.bankcard_v1.0.0.jar
	2.libbankcard_v1.0.0.so
	3.bankcardmodel

###### 注:由于3份demo文件名称重复,如果出现集成新版本,需*认真核实*应替换的文件名(包括.jar/.so/raw目录下文件/代码部分)

###### 调用和接收 [https://github.com/greenrobot/EventBus](https://github.com/greenrobot/EventBus "EventBus源码地址")

###### 注:EventBus在注册页面离开是必须解除注册,否则导致重复注册,会发生重复接收现象

###### 接收页面处理
		
	// 注册
	@Override
	public void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		// 订阅
		if (!EventBus.getDefault().isRegistered(this)) {
			EventBus.getDefault().register(this);
		}
	}

	// 反注册
	@Override
	public void onDestroyView() {
		super.onDestroyView();
		// 解除订阅
		EventBus.getDefault().unregister(this);
	}

	// 数据处理
	// 在UI线程执行
	@Subscribe(threadMode = ThreadMode.MAIN)
	public void onDataSynEvent(NewEvent event) {
		//通过对event对象内的标记进行判断(具体判断参照发送数据部分)
	}

###### 发送页面处理
	
	// post中的对象可自定义,与接收页面对应即可
	EventBus.getDefault().post(new NewEvent(Config.LIVENESS_PATH_RETURN, livenessImgPath, customer_type));

###=========================================================

### 产品计划页面
###### 1.调用对话框 (投保人,被保人,受益人类似)
	
	/** 投保人识别 */
	public void applicantOcr(final Integer type) {
		applicantAlertSfzShow(false, type);
	}
	

###### 2.左右视图对话框
	
	private void applicantAlertSfzShow(boolean isPreloading, final Integer type) {
		final AlertSfzView applicantAlertSfzView = new AlertSfzView(getActivity());
		applicantAlertSfzView.setTitle(Html.fromHtml("请拍摄身份证正面和反面<font color=\"#FF0000\">（请点击拍摄框进行拍照和重拍）</font>"));
		if (!isPreloading) {
			applicantImage = new CustomerImage();
		}
		final String idcard_applicant_1 = applicantImage.front;// policy.getAddtional(DataUtils.IDCARD_APPLICANT_1);
		final String idcard_applicant_3 = applicantImage.back;// policy.getAddtional(DataUtils.IDCARD_APPLICANT_3);
		if (OcrUtil.getDiskBitmap(idcard_applicant_1) != null) {
			applicantAlertSfzView.getIvSfzFront().setImageBitmap(OcrUtil.getDiskBitmap(idcard_applicant_1));
		}
		if (OcrUtil.getDiskBitmap(idcard_applicant_3) != null) {
			applicantAlertSfzView.getIvSfzBack().setImageBitmap(OcrUtil.getDiskBitmap(idcard_applicant_3));
		}
		applicantAlertSfzView.setOnLRClickListener(new AlertSfzView.OnLRClickListener() {

			@Override
			public void onRClick() {
				//进行授权,成功后开始识别,具体见.OcrHelp
				OcrHelp.idCardOcr(getActivity(), 1, type);
			}

			@Override
			public void onLClick() {
				OcrHelp.idCardOcr(getActivity(), 0, type);
			}
		});
		applicantAlertSfzView.setPositive(new AlertSfzView.OnClickAlertListener() {

			@Override
			public void onClick(View arg0) {
				if (OcrUtil.getDiskBitmap(idcard_applicant_1) != null
						&& OcrUtil.getDiskBitmap(idcard_applicant_3) != null) {
					showOcrShow();
					RequestNet.imageOCR(getActivity(), idcard_applicant_1, type);
					applicantAlertSfzView.dismiss();
				} else {
					ToastUtil.show(getActivity(), R.string._please_shoot);
				}
			}
		});
		applicantAlertSfzView.setNegative(new AlertSfzView.OnClickAlertListener() {

			@Override
			public void onClick(View arg0) {
				EventBus.getDefault().post(new NewEvent(Config.REMOVE, type));
			}
		});
		applicantAlertSfzView.show();
	}
	
######	3.启动授权Authorization.startIDCard做具体操作

	public static void idCardOcr(final Activity activity, final int side, final Integer type) {
		if (activity == null) {
			return;
		}
		// TODO 身份证识别
		new Authorization(activity).startIDCard(new Authorization.OnAuthorization() {

			@Override
			public void onSuccess() {
				IDCardScanActivity.show(activity, side, type, false);
			}

			@Override
			public void onFail() {
				ToastUtil.show(activity, "授权失败");
			}
		});
	}
	
###### 4.请求身份证网络授权,此处sdk会在无网络情况默认使用缓存中的授权结果(此结果保存7天sdk自动完成)
	public void startIDCard(final OnAuthorization authorization) {
		if (mProgressView == null) {
			mProgressView = new ProgressDialog(activity);
		}
		mProgressView.setMessage("正在联网授权中...");
		showProgress(true);
		new Thread(new Runnable() {
			@Override
			public void run() {
				Manager manager = new Manager(activity);
				final IDCardQualityLicenseManager idCardLicenseManager = new IDCardQualityLicenseManager(activity);
				manager.registerLicenseManager(idCardLicenseManager);
				String uuid = OcrUtil.getUUIDString(activity);
				manager.takeLicenseFromNetwork(uuid);
				String contextStr = manager.getContext(uuid);
				Log.w("ceshi", "contextStr====" + contextStr);
				Log.w("ceshi",
						"idCardLicenseManager.checkCachedLicense()===" + idCardLicenseManager.checkCachedLicense());
				if (authorization != null) {
					activity.runOnUiThread(new Runnable() {

						@Override
						public void run() {
							showProgress(false);
							if (idCardLicenseManager.checkCachedLicense() > 0) {
								authorization.onSuccess();
							} else {
								authorization.onFail();
							}
							Log.e("debug", idCardLicenseManager.checkCachedLicense() + "");
						}
					});
				}
			}
		}).start();
	}

###### 5.IDCardScanActivity.java身份证具体扫描处理类
	
	//1.调用
	/**
	 * @param side
	 *            0正面 1反面
	 * @param type
	 *            客户类型,返回时确定位置用
	 * @param isVertical
	 *            是否竖屏
	 */
	public static void show(Context context, int side, int type, boolean isVertical) {
		Intent intent = new Intent(context, IDCardScanActivity.class);
		intent.putExtra("side", side);
		intent.putExtra("customer_type", type);
		intent.putExtra("isvertical", isVertical);
		context.startActivity(intent);
	}
	
	//2.初始化raw目录下的对应文件,此处要注意替换时的名称对应,不可直接替换
	IDCardQualityAssessment.init(this, OcrUtil.readModel(this));
	
	//3.处理扫面结果
	DecodeThread.run(){
		handleSuccess(result);
	}
	
	//4.存储图片*待发送*至旷世服务器解析
	/**
	 * 保存临时图片用于上传服务器检测
	 */
	private void handleSuccess(IDCardQualityResult result) {
		//存储图片
		String image1 = OcrUtil.saveBitmap(this, "idcardimage", result.croppedImageOfIDCard());
		String image2 = null;
		//正面识别此处会返回人像的大头照片
		if (result.attr.side == IDCardAttr.IDCardSide.IDCARD_SIDE_FRONT) {
			image2 = OcrUtil.saveBitmap(this, "idcardimage", result.croppedImageOfPortrait());
		}
		IDCardImgPath imgPath = new IDCardImgPath();
		imgPath.setImage1(image1);
		imgPath.setImage2(image2);
		//发送数据
		EventBus.getDefault().post(new NewEvent(Config.IDCARD_PATH_RETURN, imgPath, customer_type));
		if (!IDCardScanActivity.this.isFinishing()) {
			IDCardScanActivity.this.finish();
		}
	}
	
	//5.因接口不支持张反面同时上传识别,又需满足需求,顾按照先正面后反面,又失败就终止的原则调用两次,对数据进行拼接得到最后结果

	// 身份证识别成功 此处开始涉及Config中的常量较多,在Config类中已做相应注释
	case Config.IDCARD_DISTINGUISH_OK:
		String success = (String) event.object;
		int additionalCode = Config.CUSTOMER_TYPE_0;
		if (event.object2 != null) {
			additionalCode = (Integer) event.object2;
			ProductEditPage.this.additionalCode = additionalCode;
		}
		JSONObject jObject;
		try {
			jObject = new JSONObject(success);
			if ("back".equals(jObject.getString("side"))) {
				// 身份证背后信息
				IDCardBackInfo backInfo = new Gson().fromJson(success, IDCardBackInfo.class);
				if (!OcrUtil.checkLegality(backInfo.getLegality())) {
					AlertView alertView = new AlertView(getActivity());
					alertView.setTitle("提示");
					alertView.setMessage("识别失败,请重新识别");
					alertView.setPositive("确定", new AlertView.OnClickAlertListener() {

						@Override
						public void onClick(View arg0) {
							if (ProductEditPage.this.additionalCode == Config.CUSTOMER_TYPE_1) {// 投保人
								applicantImage = new CustomerImage();
								applicantAlertSfzShow(true, ProductEditPage.this.additionalCode);
							} else if (ProductEditPage.this.additionalCode == Config.CUSTOMER_TYPE_0) {// 被保人
								insuredImage = new CustomerImage();
								insuredAlertSfzShow(true, ProductEditPage.this.additionalCode);
							}
						}
					});
					alertView.show();
				} else {
					if (!TextUtils.isEmpty(backInfo.getValid_date())) {
						String[] validDates = backInfo.getValid_date().split("-");
						String start = validDates[0].replace(".", "-");
						String end = "";
						boolean isL = false;
						if (validDates[1].contains("长期")) {
							isL = true;
							end = validDates[1];
						} else {
							isL = false;
							end = validDates[1].replace(".", "-");
						}
						if (customerOcrTemp != null) {
							customerOcrTemp.setIdLong(isL);
							customerOcrTemp.setIdBegDate(start);
							customerOcrTemp.setIdExpDate(end);
							AlertShowUtils.positiveInfoShow(getActivity(), customerOcrTemp, additionalCode);
						} else {
							ToastUtil.show(getActivity(), "识别信息不完整,请重新识别");
						}
					}
				}
			} else if ("front".equals(jObject.getString("side"))) {
				// 身份证正面信息
				IDCardFrontInfo frontInfo = new Gson().fromJson(success, IDCardFrontInfo.class);
				if (!OcrUtil.checkLegality(frontInfo.getLegality())) {
					AlertView alertView = new AlertView(getActivity());
					alertView.setTitle("提示");
					alertView.setMessage("识别失败,请重新识别");
					alertView.setPositive("确定", new AlertView.OnClickAlertListener() {

						@Override
						public void onClick(View arg0) {
							if (ProductEditPage.this.additionalCode == Config.CUSTOMER_TYPE_1) {// 投保人
								applicantImage = new CustomerImage();
								applicantAlertSfzShow(true, ProductEditPage.this.additionalCode);
							} else if (ProductEditPage.this.additionalCode == Config.CUSTOMER_TYPE_0) {// 被保人
								insuredImage = new CustomerImage();
								insuredAlertSfzShow(true, ProductEditPage.this.additionalCode);
							}
						}
					});
					alertView.show();
				} else {
					customerOcrTemp = new Customer();
					if (!TextUtils.isEmpty(frontInfo.getName())) {
						customerOcrTemp.setName(frontInfo.getName());
					}
					if (!TextUtils.isEmpty(frontInfo.getGender())) {
						String gender = frontInfo.getGender();
						int genderCode = 0;
						if ("男".equals(gender)) {
							genderCode = 1;
						} else if ("女".equals(gender)) {
							genderCode = 2;
						}
						customerOcrTemp.setGenderCode(genderCode);
					}
					if (frontInfo.getBirthday() != null) {
						BirthdayBean birthday = frontInfo.getBirthday();
						customerOcrTemp.setBirthday(DateUtil.getDate(
								birthday.getYear() + "-" + birthday.getMonth() + "-" + birthday.getDay()));
					}
					if (!TextUtils.isEmpty(frontInfo.getName())) {
						customerOcrTemp.setIdType(new InfoItem("1", "居民身份证"));
						customerOcrTemp.setIdNo(frontInfo.getId_card_number());// 证件号
					}
					// 正面识别成功后再识别反面
					showOcrShow();
					if (additionalCode == Config.CUSTOMER_TYPE_1) {// 投保人
						String idcard_applicant_3 = applicantImage.back;// policy.getAddtional(DataUtils.IDCARD_APPLICANT_3);
						RequestNet.imageOCR(getActivity(), idcard_applicant_3, additionalCode);
					} else if (additionalCode == Config.CUSTOMER_TYPE_0) {// 被保人
						String idcard_insured_3 = insuredImage.back;// policy.getAddtional(DataUtils.IDCARD_INSURED_3);
						RequestNet.imageOCR(getActivity(), idcard_insured_3, additionalCode);
					}
				}
			}
		} catch (JSONException e) {
			e.printStackTrace();
		}
		break;

	//6.识别完成后调用AlertShowUtils.positiveInfoShow();用户进行信息确认

	// 7.确认信息后保存临时数据并设置到页面中
	case Config.POSITIVE_INFO_OK:
		if (event.object != null) {
			int type = (Integer) event.object2;
			Customer customer = (Customer) event.object;
			if (type == Config.CUSTOMER_TYPE_0) {
				policy.setAddtional(DataUtils.CUSTOMER_INSURED_OCR_TEMP, new Gson().toJson(customer));
				policy.setAddtional(DataUtils.IDCARD_INSURED_1, insuredImage.front);
				policy.setAddtional(DataUtils.IDCARD_INSURED_2, insuredImage.bareheaded);
				policy.setAddtional(DataUtils.IDCARD_INSURED_3, insuredImage.back);
				viewQuery.id(R.id.pl_insured_name).text(customer.getName());
				viewQuery.id(R.id.pl_insured_birthday).text(DateUtil.getString(customer.getBirthday()));
				rg_insured_sex.check(customer.getGenderCode() == Customer.GENDER_FEMALE ? R.id.pl_radio_female
						: R.id.pl_radio_male);
			} else if (type == Config.CUSTOMER_TYPE_1) {
				policy.setAddtional(DataUtils.CUSTOMER_APPLICANT_OCR_TEMP, new Gson().toJson(customer));
				policy.setAddtional(DataUtils.IDCARD_APPLICANT_1, applicantImage.front);
				policy.setAddtional(DataUtils.IDCARD_APPLICANT_2, applicantImage.bareheaded);
				policy.setAddtional(DataUtils.IDCARD_APPLICANT_3, applicantImage.back);
				viewQuery.id(R.id.pl_applicant_name).text(customer.getName());
				viewQuery.id(R.id.pl_applicant_birthday).text(DateUtil.getString(customer.getBirthday()));
				rg_applicant_sex.check(customer.getGenderCode() == Customer.GENDER_FEMALE ? R.id.pl_radio_female
						: R.id.pl_radio_male);
			}
		}
		break;

### 回执回销页面(银行转账页面类似)
	
	//1.调用人脸识别
	public void sign() {
		getActivity().runOnUiThread(new Runnable() {
			@Override
			public void run() {
				LogUtil.w("FACE", "调用人脸识别");
				startFace(com.n22.ocr.util.Config.SEARCH_RECEIPT_0);
			}
		});
	}
	
	//2.授权并提示选择横竖屏幕识别
	private void startFace(final int type) {
		// TODO 人脸识别
		new Authorization(getActivity()).startLive(new Authorization.OnAuthorization() {

			@Override
			public void onSuccess() {
				ToastUtil.show(getActivity(), "授权成功");
				String[] items = { "竖屏识别", "横屏识别" };
				AlertDialog.Builder builder = new Builder(getActivity());
				builder.setTitle("提示");
				builder.setItems(items, new AlertDialog.OnClickListener() {

					@Override
					public void onClick(DialogInterface dialog, int which) {
						if (which == 0) {
							LivenessActivity.show(getActivity(), type, true);
						} else if (which == 1) {
							LivenessActivity.show(getActivity(), type, false);
						}
					}
				});
				builder.show();
			}

			@Override
			public void onFail() {
				ToastUtil.show(getActivity(), "授权失败");
			}
		});

	}

	
	// 3.识别完成 在UI线程执行
	@Subscribe(threadMode = ThreadMode.MAIN)
	public void onDataSynEvent(NewEvent event) {
		if (event != null) {
			switch (event.position) {
			// 人脸识别返回结果
			case com.n22.ocr.util.Config.LIVENESS_DISTINGUISH_RETURN:

				String data = (String) event.object;
				int additionalCode = com.n22.ocr.util.Config.SEARCH_RECEIPT_0;
				if (event.object2 != null) {
					additionalCode = (Integer) event.object2;
				}
				try {
					JSONObject result = new JSONObject(data);
					int resID = result.getInt("resultcode");
					if (resID == R.string.verify_success) {
						warnDialog = new AlertView(getActivity()).setCancelable(false).showProgress().show();
						warnDialog.setTitle("提示").setMessage("人像采集成功,感谢您的配合...");
						OcrUtil.doPlay(getActivity(), R.raw.meglive_success, new Completion(additionalCode, true));
					} else if (resID == R.string.liveness_detection_failed_not_video) {
						OcrUtil.doPlay(getActivity(), R.raw.meglive_failed);
					} else if (resID == R.string.liveness_detection_failed_timeout) {
						OcrUtil.doPlay(getActivity(), R.raw.meglive_failed);
					} else if (resID == R.string.liveness_detection_failed) {
						OcrUtil.doPlay(getActivity(), R.raw.meglive_failed);
					} else {
						OcrUtil.doPlay(getActivity(), R.raw.meglive_failed);
					}
				} catch (JSONException e) {
					e.printStackTrace();
				}
				break;
			// 识别中获取的最优照片
			case com.n22.ocr.util.Config.LIVENESS_PATH_RETURN:
				LivenessImgInfo imgPath = (LivenessImgInfo) event.object;
				if (!TextUtils.isEmpty(imgPath.getImage())) {
					if (event.object2 != null) {
						LogUtil.w("FACE", "人脸照片采集成功");
						additionalCode = (Integer) event.object2;
						if (additionalCode == com.n22.ocr.util.Config.SEARCH_RECEIPT_0) {
							mImgPath = imgPath;
						}
					}
				}
				break;
			}
		}
	}

	//4.采集成功后接应用原签字功能

###### 注:根据startFace(value);传入的参数不同可应用与,转账授权书,电子保单等签字页面

###==========================================================

### 接口部分
	
##### OCR影像件上传规则设计
	
	影像件上传接口增加节点----isPayAccUpdateProcess(是否支付账户修改流程)

	当pad调用影像件上传接口时，服务端判断isPayAccUpdateProcess节点值：
	
	（1）如果为N，即正常流程的影像件上传接口，服务端不作任何修改；
	
	（2）如果为Y，证明此次接口调用是在银行转账授权书流程中发起，服务端判断前端所传图片流列	表中是否包含889（其他资料）类型的照片，即OCR银行卡照片；

#####人脸比对接口接入
###### 注意事项(已在ProductEditPage的next()中添加)
	基于只有活脸识别上线后出的保单才需要人脸比对，故服务端需要一个标识证明代理人正在使用的版本为活脸识别上线后的版本，前端可在policy的additional中传值，传值规则如下：
	Key：isMegLive
	Value：Y
	此节点未生成或除Y之外的值，服务端均认为代理人使用的版本为非活脸识别版本
