---
layout: post
title: C# 自定义 下拉框
category: cs
tags: [C#, combobox]
keywords: C#, combobox
description: C#自定义设置边框下拉框
---

<pre class="prettyprint linenums">
using System;
using System.Collections.Generic;
using System.Text;
using System.Windows.Forms;
using System.Drawing;

namespace Custom
{
    public class CustomComBoBox : ComboBox
    {
        private Color borderColor;

        /// <summary>
        /// 边框颜色
        /// </summary>
        public Color BorderColor
        {
            get { return borderColor; }
            set { borderColor = value; }
        }

        protected override void WndProc(ref Message m)
        {
            base.WndProc(ref m);
            ControlPaint.DrawBorder(this.CreateGraphics(), this.ClientRectangle, this.BorderColor, ButtonBorderStyle.Solid);
        } 
    }
}
</pre>
