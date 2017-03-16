package com.zlyjkj.customerservice.fragment;

import android.app.Dialog;
import android.os.Bundle;
import android.support.annotation.Nullable;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;
import android.widget.Button;
import android.widget.ImageView;
import android.widget.LinearLayout;
import android.widget.TextView;

import com.zlyjkj.customerservice.R;
import com.zlyjkj.customerservice.login.ForgetPasswordActivity;
import com.zlyjkj.customerservice.login.LoginOneActivity;
import com.zlyjkj.customerservice.threeactivity.ResetPasswordActivity;
import com.zlyjkj.kehu.base.json.MyRegisterJsonPackage;
import com.zlyjkj.kehu.base.main.MyFragment;
import com.zlyjkj.kehu.base.my.MyArrayActivity;
import com.zlyjkj.kehu.base.my.MyClassFund;
import com.zlyjkj.kehu.base.my.MyDownApk;
import com.zlyjkj.kehu.base.my.MyInterface;
import com.zlyjkj.kehu.base.my.MyTitleDialog;
import com.zlyjkj.kehu.mode.User;

import org.json.JSONArray;
import org.json.JSONException;
import org.xutils.view.annotation.ContentView;
import org.xutils.view.annotation.ViewInject;
import org.xutils.x;

@ContentView(R.layout.fragment_person_center)
public class PersonCenterFragment extends MyFragment {
    private View mContentView;

    @ViewInject(R.id.iv_head_icon_person_center)
    private ImageView mImgHead; //头像

    @ViewInject(R.id.tv_name_person_center)
    private TextView mTxtName; //昵称

    @ViewInject(R.id.ll_forget_pswd_person_center)
    private LinearLayout mLinearForgetPswd; //忘记密码

    @ViewInject(R.id.ll_reset_pswd_person_center)
    private LinearLayout mLinearResetPswd; //重置密码

    @ViewInject(R.id.ll_feedback_person_center)
    private LinearLayout mLinearFeedback; //意见反馈

    @ViewInject(R.id.ll_wape_cache_pswd_person_center)
    private LinearLayout mLinearWapeCache; //清空缓存

    @ViewInject(R.id.ll_version_detection_person_center)
    private LinearLayout mLinearVersionDete; //版本检测

    @ViewInject(R.id.ll_phone_person_center)
    private LinearLayout mLinearPhone; //电话

    @ViewInject(R.id.ll_recovery_managent_person_center)
    private LinearLayout mLinearRecoveryManagent; //回复管理

    @ViewInject(R.id.btn_cancel_login_person_center)
    private Button mBtnCancelLogin;

    public PersonCenterFragment() {
    }

    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
    }

    @Override
    public View onCreateView(LayoutInflater inflater, ViewGroup container,
                             Bundle savedInstanceState) {
//        if (mContentView == null) {
//            mContentView = inflater.inflate(R.layout.fragment_person_center, container, false);
//        }
//
//        ViewGroup viewGroup = (ViewGroup) mContentView.getParent();
//        if (viewGroup != null) {
//            viewGroup.removeAllViews();
//        }
        return x.view().inject(this,inflater,container);
    }

    @Override
    public void onActivityCreated(@Nullable Bundle savedInstanceState) {
        super.onActivityCreated(savedInstanceState);

        MyClassFund.setUserInformation(mImgHead,mTxtName,null);

        viewClickListener();
    }

    private void viewClickListener() {
        //跳转到忘记密码界面
        mLinearForgetPswd.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                MyArrayActivity.skipToActivity(getActivity(), ForgetPasswordActivity.class);
            }
        });

        //跳转到密码重置界面
        mLinearResetPswd.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                MyArrayActivity.skipToActivity(getActivity(), ResetPasswordActivity.class);
            }
        });

        //意见反馈
        // 清空缓存
        // 版本检测
        mLinearVersionDete.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                MyRegisterJsonPackage.checkVersion(getActivity(), new MyInterface.networkResult() {
                    @Override
                    public void resultSuccess(JSONArray aJsonArray, String aStrMessage) throws JSONException {
                        final String url = aJsonArray.getJSONObject(0).getString("url");
                        MyTitleDialog.show(getActivity(), false, aJsonArray.getJSONObject(0).getString("appname"),
                                aJsonArray.getJSONObject(0).getString("vername"), new MyInterface.dialogButtonClick() {
                                    @Override
                                    public void okClickListener(Dialog aDialog) {
                                        MyDownApk.startDown(getActivity(), url);
                                        aDialog.dismiss();
                                    }

                                    @Override
                                    public void cancelClickListener(Dialog aDialog) {
                                        aDialog.dismiss();
                                    }
                                });
                    }

                    @Override
                    public void resultFailed(String aStrMessage) {
                        MyTitleDialog.show(getActivity(), false, "", aStrMessage, null);
                    }

                    @Override
                    public void resultFinish() {
                    }
                });
            }
        });
        // 电话
        // 回复管理

        //注销登录
        mBtnCancelLogin.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                MyTitleDialog.show(getActivity(), false, "", getString(R.string.your_sure_exit_login), new MyInterface.dialogButtonClick() {
                    @Override
                    public void okClickListener(Dialog aDialog) {
                        User.clearUserInfo();
                        MyArrayActivity.clearAllActivity();
                        MyArrayActivity.skipToActivity(getActivity(), LoginOneActivity.class,"flag");
                        aDialog.dismiss();
                        getActivity().finish();
                    }

                    @Override
                    public void cancelClickListener(Dialog aDialog) {
                        aDialog.dismiss();
                    }
                });
            }
        });
    }
}

