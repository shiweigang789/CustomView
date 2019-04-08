#### Paint

    Paint mPaint = new Paint(); //初始化
    mPaint.setColor(Color.RED);// 设置颜色
    mPaint.setARGB(255, 255, 255, 0); // 设置 Paint对象颜色,范围为0~255
    mPaint.setAlpha(200); // 设置alpha不透明度,范围为0~255
    mPaint.setAntiAlias(true); // 抗锯齿
    mPaint.setStyle(Paint.Style.FILL); //描边效果
    mPaint.setStrokeWidth(4);//描边宽度
    mPaint.setStrokeCap(Paint.Cap.ROUND); //圆角效果
        Paint.Cap.ROUND
        Paint.Cap.BUTT
        Paint.Cap.SQUARE
    mPaint.setStrokeJoin(Paint.Join.MITER);//拐角风格
        Paint.Join.MITER
        Paint.Join.BEVEL
        Paint.Join.ROUND
    mPaint.setShader(new SweepGradient(200, 200, Color.BLUE, Color.RED)); //设置环形渲染器
    mPaint.setXfermode(new PorterDuffXfermode(PorterDuff.Mode.DARKEN)); //设置图层混合模式
    mPaint.setColorFilter(new LightingColorFilter(0x00ffff, 0x000000)); //设置颜色过滤器
    mPaint.setFilterBitmap(true); //设置双线性过滤
    mPaint.setMaskFilter(new BlurMaskFilter(10, BlurMaskFilter.Blur.NORMAL));//设置画笔遮罩滤镜 ,传入度数和样式
    mPaint.setTextScaleX(2);// 设置文本缩放倍数
    mPaint.setTextSize(38);// 设置字体大小
    mPaint.setTextAlign(Paint.Align.LEFT);//对其方式
    mPaint.setUnderlineText(true);// 设置下划线
    Rect rect = new Rect();
    mPaint.getTextBounds(str, 0, str.length(), rect); //测量文本大小，将文本大小信息存放在rect中
    mPaint.measureText(str); //获取文本的宽
    mPaint.getFontMetrics(); //获取字体度量对象
    
    
    渲染器
    Shader mShader 
    
    
    /**
     * 1.线性渲染,LinearGradient(float x0, float y0, float x1, float y1, 
     * @NonNull @ColorInt int colors[], @Nullable float positions[], @NonNull TileMode tile)
     * (x0,y0)：渐变起始点坐标
     * (x1,y1):渐变结束点坐标
     * color0:渐变开始点颜色,16进制的颜色表示，必须要带有透明度
     * color1:渐变结束颜色
     * colors:渐变数组
     * positions:位置数组，position的取值范围[0,1],作用是指定某个位置的颜色值，如果传null，渐变就线性变化。
     * tile:用于指定控件区域大于指定的渐变区域时，空白区域的颜色填充方法
     */
    mShader = new LinearGradient(0,0,500,500,Color.RED, Color.BLUE,Shader.TileMode.REPEAT);
    mShader = new LinearGradient(0, 0, 500, 500, new int[]{Color.RED, Color.BLUE, Color.GREEN}, 
    new float[]{0.f,0.7f,1}, Shader.TileMode.REPEAT);
    mPaint.setShader(mShader);
    
    
    /**
     * 环形渲染，RadialGradient(float centerX, float centerY, float radius, @ColorInt int colors[], 
     * @Nullable float stops[], TileMode tileMode)
     * centerX ,centerY：shader的中心坐标，开始渐变的坐标
     * radius:渐变的半径
     * centerColor,edgeColor:中心点渐变颜色，边界的渐变颜色
     * colors:渐变颜色数组
     * stoops:渐变位置数组，类似扫描渐变的positions数组，取值[0,1],中心点为0，半径到达位置为1.0f
     * tileMode:shader未覆盖以外的填充模式。
     */
    mShader = new RadialGradient(250, 250, 250,Color.GREEN, Color.YELLOW,Shader.TileMode.CLAMP);
    mShader = new RadialGradient(250, 250, 250, new int[]{Color.GREEN, Color.YELLOW, Color.RED}, 
    new float[]{0f, 0.8f, 1f}, Shader.TileMode.CLAMP);
    mPaint.setShader(mShader);
    
    
    /**
      * 扫描渲染，SweepGradient(float cx, float cy, @ColorInt int color0,int color1)
      * cx,cy 渐变中心坐标
      * color0,color1：渐变开始结束颜色
      * colors，positions：类似LinearGradient,用于多颜色渐变,positions为null时，根据颜色线性渐变
      */
    mShader = new SweepGradient(250, 250, new int[]{Color.GREEN, Color.YELLOW, Color.RED}, new float[]{0f, 0.8f, 1f});
    mShader = new SweepGradient(250, 250, Color.RED, Color.GREEN);
    mPaint.setShader(mShader);
    
    
    /**
      * 位图渲染，BitmapShader(@NonNull Bitmap bitmap, @NonNull TileMode tileX, @NonNull TileMode tileY)
      * Bitmap:构造shader使用的bitmap
      * tileX：X轴方向的TileMode
      * tileY:Y轴方向的TileMode
      * REPEAT, 绘制区域超过渲染区域的部分，重复排版
      * CLAMP， 绘制区域超过渲染区域的部分，会以最后一个像素拉伸排版
      * MIRROR, 绘制区域超过渲染区域的部分，镜像翻转排版
      */
    mShader = new BitmapShader(mBitmap, Shader.TileMode.REPEAT, Shader.TileMode.MIRROR);
    mPaint.setShader(mShader);
    
    
    /**
      * 组合渲染，
      * ComposeShader(@NonNull Shader shaderA, @NonNull Shader shaderB, Xfermode mode)
      * ComposeShader(@NonNull Shader shaderA, @NonNull Shader shaderB, PorterDuff.Mode mode)
      * shaderA,shaderB:要混合的两种shader
      * Xfermode mode： 组合两种shader颜色的模式
      * PorterDuff.Mode mode: 组合两种shader颜色的模式
      */
    BitmapShader bitmapShader = new BitmapShader(mBitmap, Shader.TileMode.REPEAT, Shader.TileMode.REPEAT);
    LinearGradient linearGradient = new LinearGradient(0, 0, 1000, 1600, 
    new int[]{Color.RED, Color.GREEN, Color.BLUE}, null,       Shader.TileMode.CLAMP);
    mShader = new ComposeShader(bitmapShader, linearGradient, new Xfermode());
    mShader = new ComposeShader(bitmapShader, linearGradient, PorterDuff.Mode.MULTIPLY);
    mPaint.setShader(mShader);


    xfermode
    Bitmap mBitmap = BitmapFactory.decodeResource(getResources(), R.mipmap.girl);
    ColorMatrixColorFilter mColorMatrixColorFilter

     /**
       * R' = R * mul.R / 0xff + add.R
       * G' = G * mul.G / 0xff + add.G
       * B' = B * mul.B / 0xff + add.B
       */

     //原始图片效果
     LightingColorFilter lighting = new LightingColorFilter(0xffffff,0x000000);
     mPaint.setColorFilter(lighting);
     canvas.drawBitmap(mBitmap, 0,0, mPaint);

     //红色去除掉
     LightingColorFilter lighting = new LightingColorFilter(0x00ffff,0x000000);
     mPaint.setColorFilter(lighting);
     canvas.drawBitmap(mBitmap, 0,0, mPaint);

     //绿色更亮
     LightingColorFilter lighting = new LightingColorFilter(0xffffff,0x003000);
     mPaint.setColorFilter(lighting);
     canvas.drawBitmap(mBitmap, 0,0, mPaint);

     PorterDuffColorFilter porterDuffColorFilter = new PorterDuffColorFilter(Color.RED, PorterDuff.Mode.DARKEN);
     mPaint.setColorFilter(porterDuffColorFilter);
     canvas.drawBitmap(mBitmap, 100, 0, mPaint);

     float[] colorMatrix = {
                     2,0,0,0,0,   //red
                     0,1,0,0,0,   //green
                     0,0,1,0,0,   //blue
                     0,0,0,1,0    //alpha
             };
     mColorMatrixColorFilter = new ColorMatrixColorFilter(colorMatrix);
     mPaint.setColorFilter(mColorMatrixColorFilter);
     canvas.drawBitmap(mBitmap, 100, 0, mPaint);

     ColorMatrix cm = new ColorMatrix();
     //亮度调节
     cm.setScale(1,2,1,1);
     //饱和度调节0-无色彩， 1- 默认效果， >1饱和度加强
     cm.setSaturation(2);
     //色调调节
     cm.setRotate(0, 45);
     mColorMatrixColorFilter = new ColorMatrixColorFilter(cm);
     mPaint.setColorFilter(mColorMatrixColorFilter);
     canvas.drawBitmap(mBitmap, 100, 0, mPaint);


#### Canvas

      /**
        * 1.canvas内部对于状态的保存存放在栈中
        * 2.可以多次调用save保存canvas的状态，并且可以通过getSaveCount方法获取保存的状态个数
        * 3.可以通过restore方法返回最近一次save前的状态，
        * 也可以通过restoreToCount返回指定save状态。指定save状态之后的状态全部被清除
        * 4.saveLayer可以创建新的图层，之后的绘制都会在这个图层之上绘制，直到调用restore方法
        * 注意：绘制的坐标系不能超过图层的范围， saveLayerAlpha对图层增加了透明度信息
        */

      Log.e("", "onDraw: "+canvas.getSaveCount());
      canvas.drawRect(200, 200, 700, 700, mPaint);
      int state = canvas.save();
      Log.e("", "onDraw: "+canvas.getSaveCount());
      canvas.translate(50,50);

      mPaint.setColor(Color.GRAY);
      canvas.drawRect(0,0,500,500, mPaint);

      canvas.save();
      Log.e("", "onDraw: "+canvas.getSaveCount());
      canvas.translate(50,50);
      mPaint.setColor(Color.BLUE);
      canvas.drawRect(0,0,500,500, mPaint);


      canvas.restore();
      Log.e("", "onDraw: "+canvas.getSaveCount());
      canvas.restore();
      canvas.restoreToCount(state);

      canvas.restore();
      Log.e("", "onDraw: "+canvas.getSaveCount());
      canvas.drawLine(0,0, 400,500, mPaint);

      canvas.drawRect(200,200, 700,700, mPaint);

      int layerId = canvas.saveLayer(0,0, 700, 700, mPaint);
      mPaint.setColor(Color.GRAY);
      Matrix matrix = new Matrix();
      matrix.setTranslate(100,100);
      canvas.setMatrix(matrix);
      //由于平移操作，导致绘制的矩形超出了图层的大小，所以绘制不完全
      canvas.drawRect(0,0,700,700, mPaint);
      canvas.restoreToCount(layerId);

      mPaint.setColor(Color.RED);
      canvas.drawRect(0,0,100,100, mPaint);

      //1，平移操作
      canvas.drawRect(0,0, 400, 400, mPaint);
      canvas.translate(50, 50);
      mPaint.setColor(Color.GRAY);
      canvas.drawRect(0,0, 400, 400, mPaint);
      canvas.drawLine(0, 0, 600,600, mPaint);

      //缩放操纵
      canvas.drawRect(200,200, 700,700, mPaint);
      canvas.scale(0.5f, 0.5f);
      //先translate(px, py),再scale(sx, sy),再反响translate
      canvas.scale(0.5f, 0.5f, 200,200);

      canvas.translate(200, 200);
      canvas.scale(0.5f, 0.5f);
      canvas.translate(-200, -200);

      mPaint.setColor(Color.GRAY);
      canvas.drawRect(200,200, 700,700, mPaint);
      canvas.drawLine(0,0, 400, 600, mPaint);

      //旋转操作
      canvas.translate(50,50);
      canvas.drawRect(0,0, 700,700, mPaint);
      canvas.rotate(45);
      mPaint.setColor(Color.GRAY);
      canvas.drawRect(0,0, 700,700, mPaint);

      canvas.drawRect(400, 400, 900, 900, mPaint);
      canvas.rotate(45, 650, 650); //px, py表示旋转中心的坐标
      mPaint.setColor(Color.GRAY);
      canvas.drawRect(400, 400, 900, 900, mPaint);


      //倾斜操作
      canvas.drawRect(0,0, 400, 400, mPaint);
      canvas.skew(1, 0); //在X方向倾斜45度,Y轴逆时针旋转45
      canvas.skew(0, 1); //在y方向倾斜45度， X轴顺时针旋转45
      mPaint.setColor(Color.GRAY);
      canvas.drawRect(0, 0, 400, 400, mPaint);

      //切割
      canvas.drawRect(200, 200,700, 700, mPaint);
      mPaint.setColor(Color.GRAY);
      canvas.drawRect(200, 800,700, 1300, mPaint);
      canvas.clipRect(200, 200,700, 700); //画布被裁剪
      canvas.drawCircle(100,100, 100,mPaint); //坐标超出裁剪区域，无法绘制
      canvas.drawCircle(300, 300, 100, mPaint); //坐标区域在裁剪范围内，绘制成功

      canvas.drawRect(200, 200,700, 700, mPaint);
      mPaint.setColor(Color.GRAY);
      canvas.drawRect(200, 800,700, 1300, mPaint);
      canvas.clipOutRect(200,200,700,700); //画布裁剪外的区域
      canvas.drawCircle(100,100,100,mPaint); //坐标区域在裁剪范围内，绘制成功
      canvas.drawCircle(300, 300, 100, mPaint);//坐标超出裁剪区域，无法绘制

      //矩阵
      canvas.drawRect(0, 0, 700, 700, mPaint);
      Matrix matrix = new Matrix();
      matrix.setTranslate(50, 50);
      matrix.setRotate(45);
      matrix.setScale(0.5f, 0.5f);
      canvas.setMatrix(matrix);
      mPaint.setColor(Color.GRAY);
      canvas.drawRect(0, 0, 700, 700, mPaint);