```Java
package com.ui.music;

import static android.media.AudioManager.AUDIOFOCUS_REQUEST_GRANTED;

import android.app.Notification;
import android.app.NotificationChannel;
import android.app.NotificationManager;
import android.app.PendingIntent;
import android.content.Context;
import android.content.Intent;
import android.content.IntentFilter;
import android.media.AudioAttributes;
import android.media.AudioFocusRequest;
import android.media.AudioManager;
import android.text.Editable;
import android.text.TextWatcher;
import android.util.Log;
import android.view.View;
import android.view.ViewGroup;
import android.widget.AdapterView;
import android.widget.BaseAdapter;
import android.widget.RemoteViews;

import androidx.core.app.NotificationCompat;

import com.bumptech.glide.Glide;
import com.ui.music.databinding.ActivityMusicListBinding;
import com.ui.music.databinding.ItemMusicBinding;

import org.greenrobot.eventbus.EventBus;
import org.greenrobot.eventbus.Subscribe;
import org.greenrobot.eventbus.ThreadMode;

import java.util.ArrayList;
import java.util.List;
import java.util.Locale;

public class MusicListActivity extends BaseActivity {
    ActivityMusicListBinding binding;
    List<MusicInfo> baseMusics = new ArrayList<>();

    @Override
    protected View getRoot() {
        binding = ActivityMusicListBinding.inflate(getLayoutInflater());
        return binding.getRoot();
    }

    @Override
    protected void initListener() {
        binding.etSearch.addTextChangedListener(new TextWatcher() {
            @Override
            public void beforeTextChanged(CharSequence charSequence, int i, int i1, int i2) {

            }

            @Override
            public void onTextChanged(CharSequence charSequence, int i, int i1, int i2) {

            }

            @Override
            public void afterTextChanged(Editable editable) {
                String trim = editable.toString().trim();
                List<MusicInfo> collection = MusicHelper.getLocalMusic(MusicListActivity.this);
                baseMusics.clear();
                if (trim.isEmpty()) {
                    baseMusics.addAll(collection);
                } else {
                    for (MusicInfo baseMusic : collection) {
                        if (baseMusic.getTitle().contains(trim)) {
                            baseMusics.add(baseMusic);
                        }
                    }
                }
                adapter.notifyDataSetChanged();
            }
        });


    }

    MusicAdapter adapter = new MusicAdapter();

    @Override
    protected void initData() {
        IntentFilter filter = new IntentFilter();
        filter.addAction("last");
        filter.addAction("pause");
        filter.addAction("next");
        registerReceiver(new MusicReceiver(), filter);

        baseMusics.addAll(MusicHelper.getLocalMusic(this));
        binding.lvMusic.setAdapter(adapter);
        binding.lvMusic.setOnItemClickListener(new AdapterView.OnItemClickListener() {
            @Override
            public void onItemClick(AdapterView<?> adapterView, View view, int i, long l) {
                isPlaying = true;
                index = i;
                setMusicInfo();
            }
        });
    }

    boolean pause = false;

    @Override
    protected void onPause() {
        super.onPause();
        pause = true;
        EventBus.getDefault().unregister(this);
    }


    @Subscribe(threadMode = ThreadMode.MAIN)
    public void onReceive(String action) {
        if (pause) return;
        Log.d("TAG", "MusicList onReceive: " + action);
        if (action.equals("next")) {
            index++;
            if (index <= baseMusics.size() - 1) {
                MusicService.getInstance().setDataSource(this, baseMusics.get(index).getMusicUri());
            }
        } else if (action.equals("last")) {
            index--;
            if (index >= 0 && !baseMusics.isEmpty()) {
                MusicService.getInstance().setDataSource(this, baseMusics.get(index).getMusicUri());
            }
        } else if (action.equals("pause")) {
            isPlaying = !isPlaying;

            if (isPlaying) {
                binding.ivPause.setImageResource(R.drawable.ic_baseline_pause_24);
//                animator.resume();
                MusicService.getInstance().play();
            } else {
                binding.ivPause.setImageResource(R.drawable.ic_baseline_play_arrow_24);
//                animator.pause();
                MusicService.getInstance().pause();
            }


        }
        Log.d("TAG", "onReceive: " + index);
        Glide.with(binding.getRoot()).load(baseMusics.get(index).getAlbumArtUri()).error(R.drawable.ic_music).into(binding.ivImage);
        setNotification(baseMusics.get(index));
    }

    public int index = 0;

    @Override
    protected void findViewsById() {
        binding.ivLast.setOnClickListener(v -> {
            index--;
            if (index >= 0 && !baseMusics.isEmpty()) {
                MusicService.getInstance().setDataSource(MusicListActivity.this, baseMusics.get(index).getMusicUri());
            }
            setMusicInfo();
        });
        binding.ivPause.setOnClickListener(v -> {
            isPlaying = !isPlaying;
            if (isPlaying) {
                binding.ivPause.setImageResource(R.drawable.ic_baseline_pause_24);
                MusicService.getInstance().play();
            } else {
                binding.ivPause.setImageResource(R.drawable.ic_baseline_play_arrow_24);
                MusicService.getInstance().pause();
            }

            MusicInfo musicInfo = baseMusics.get(index);
            Glide.with(this).load(musicInfo.getAlbumArtUri()).error(R.drawable.ic_music).into(binding.ivImage);
            binding.tvName.setText(musicInfo.getTitle());
            setNotification(musicInfo);

//            setMusicInfo();
        });
        binding.ivNext.setOnClickListener(v -> {
            index++;
            if (index <= baseMusics.size() - 1) {
                MusicService.getInstance().setDataSource(this, baseMusics.get(index).getMusicUri());
                setMusicInfo();
            }
        });
        binding.bottom.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                startActivity(new Intent(MusicListActivity.this, MusicActivity.class).putExtra("index", index));
            }
        });
    }

    private void setMusicInfo() {
        MusicInfo musicInfo = baseMusics.get(index);
        MusicService.getInstance().setDataSource(this, musicInfo.getMusicUri());
        Glide.with(this).load(musicInfo.getAlbumArtUri()).error(R.drawable.ic_music).into(binding.ivImage);
        binding.tvName.setText(musicInfo.getTitle());
        setNotification(musicInfo);
    }

    private RemoteViews remoteViews;
    private boolean isPlaying = false;
    public static int focus = 1;

    @Override
    protected void onResume() {
        super.onResume();
        Log.d("TAG", "onResume: ");
        pause = false;

        EventBus.getDefault().register(this);
        AudioManager audioManager = (AudioManager) getSystemService(Context.AUDIO_SERVICE);
        AudioAttributes.Builder attributes = new AudioAttributes.Builder();
        attributes.setContentType(AudioAttributes.CONTENT_TYPE_MUSIC).setUsage(AudioAttributes.USAGE_MEDIA);
        AudioFocusRequest.Builder request = new AudioFocusRequest.Builder(AudioManager.AUDIOFOCUS_GAIN);
        request.setAudioAttributes(attributes.build()).setOnAudioFocusChangeListener(listener);

        focus = audioManager.requestAudioFocus(request.build());
        if (focus == AUDIOFOCUS_REQUEST_GRANTED) {
            MusicService.getInstance().play();
        }
        Log.d("TAG", "onResume: " + focus);
        MusicInfo musicInfo = baseMusics.get(index);
//        MusicService.getInstance().setDataSource(this, musicInfo.getMusicUri());
        Glide.with(this).load(musicInfo.getAlbumArtUri()).error(R.drawable.ic_music).into(binding.ivImage);
        binding.tvName.setText(musicInfo.getTitle());
        setNotification(musicInfo);

    }

    public static AudioManager.OnAudioFocusChangeListener listener = new AudioManager.OnAudioFocusChangeListener() {
        @Override
        public void onAudioFocusChange(int focusChange) {
            switch (focusChange) {
                case AudioManager.AUDIOFOCUS_GAIN:
                    Log.d("TAG", "onAudioFocusChange: 获得");
                    MusicService.getInstance().play();
                    focus = 1;
                    break;
                case AudioManager.AUDIOFOCUS_LOSS:
                    // 当应用程序永久失去焦点时，应该停止播放音频并释放资源
                    Log.d("TAG", "onAudioFocusChange: 永久失去焦点时");
                    MusicService.getInstance().pause();
                    focus = -1;
                    break;
                case AudioManager.AUDIOFOCUS_LOSS_TRANSIENT:
                    Log.d("TAG", "onAudioFocusChange: 短暂失去");
                    // 当应用程序瞬时失去焦点时，应该暂停播放音频
                    MusicService.getInstance().pause();
                    focus = -1;
                    break;
                // 其他焦点状态处理...
            }
        }
    };

    @Subscribe
    public void onChange(Integer index) {
        this.index = index;
        MusicInfo musicInfo = baseMusics.get(index);
        Glide.with(this).load(musicInfo.getAlbumArtUri()).error(R.drawable.ic_music).into(binding.ivImage);
        binding.tvName.setText(musicInfo.getTitle());
        setNotification(musicInfo);
    }

    /**
     * 设置通知栏
     */
    public void setNotification(MusicInfo musicInfo) {
        NoticeUtil.setNotification(musicInfo, isPlaying);
//        remoteViews = new RemoteViews(getPackageName(), R.layout.notice_view);
//        //设置文字
//        remoteViews.setTextViewText(R.id.tv_title, musicInfo.getTitle());
//        remoteViews.setImageViewUri(R.id.iv_image, musicInfo.getAlbumArtUri());
//        if (isPlaying) {
//            remoteViews.setImageViewResource(R.id.iv_pause, R.drawable.ic_baseline_pause_24);
//        } else {
//            remoteViews.setImageViewResource(R.id.iv_pause, R.drawable.ic_baseline_play_arrow_24);
//        }
//        //上一首
//        Intent previous = new Intent("last");
//        PendingIntent pi_previous = PendingIntent.getBroadcast(this, 0, previous, PendingIntent.FLAG_UPDATE_CURRENT);
//        remoteViews.setOnClickPendingIntent(R.id.iv_last, pi_previous);
//
//
//        //播放
//        Intent play = new Intent("pause");
//        PendingIntent pi_play = PendingIntent.getBroadcast(this, 0, play, PendingIntent.FLAG_UPDATE_CURRENT);
//        remoteViews.setOnClickPendingIntent(R.id.iv_pause, pi_play);
//
//
//        //下一首
//        Intent next = new Intent("next");
//        PendingIntent pi_next = PendingIntent.getBroadcast(this, 0, next, PendingIntent.FLAG_UPDATE_CURRENT);
//        remoteViews.setOnClickPendingIntent(R.id.iv_next, pi_next);
//
//
//        NotificationChannel channel;
//        NotificationManager manager = (NotificationManager) getSystemService(Context.NOTIFICATION_SERVICE);
//        String channelId = "chat";
//        String channelName = "音乐";
//        int importance = NotificationManager.IMPORTANCE_HIGH;
//        channel = new NotificationChannel(channelId, channelName, importance);
//        NotificationManager notificationManager = (NotificationManager) getSystemService(NOTIFICATION_SERVICE);
//        notificationManager.createNotificationChannel(channel);
//
//
//        Notification notification = new NotificationCompat.Builder(this, "chat").setCustomContentView(remoteViews)
////                .setContentIntent(pendingIntent)
//                .setWhen(System.currentTimeMillis()).setSmallIcon(R.mipmap.ic_launcher).setAutoCancel(true).build();
//        manager.notify(1, notification);

    }

    private class MusicAdapter extends BaseAdapter {
        @Override
        public int getCount() {
            return baseMusics.size();
        }

        @Override
        public MusicInfo getItem(int i) {
            return baseMusics.get(i);
        }

        @Override
        public long getItemId(int i) {
            return i;
        }

        @Override
        public View getView(int i, View view, ViewGroup viewGroup) {
            ItemMusicBinding binding;
            if (view == null) {
                binding = ItemMusicBinding.inflate(getLayoutInflater(), viewGroup, false);
                view = binding.getRoot();
                view.setTag(binding);
            } else {
                binding = (ItemMusicBinding) view.getTag();
            }
            binding.name.setText(baseMusics.get(i).getTitle() + "-" + baseMusics.get(i).getAlbum());
            int seconds = (int) (baseMusics.get(i).getDuration() / 1000);
            int hours = seconds / 3600;
            int minutes = (seconds % 3600) / 60;
            int remainingSeconds = seconds % 60;
            binding.duration.setText(String.format(Locale.getDefault(), "%02d:%02d", minutes, remainingSeconds));
            return binding.getRoot();
        }
    }

}
```