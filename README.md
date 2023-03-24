# Joytick_test1


package be.nabil.jeujeu

import android.content.Context
import android.graphics.Canvas
import android.graphics.Color
import android.graphics.Paint
import android.graphics.PointF
import android.util.AttributeSet
import android.view.MotionEvent
import android.view.View

class Joystick @JvmOverloads constructor(
    context: Context,
    attrs: AttributeSet? = null,
    defStyleAttr: Int = 0
) : View(context, attrs, defStyleAttr) {

    private val paint = Paint().apply {
        color = Color.BLACK
        style = Paint.Style.FILL
    }

    private val center = PointF()
    private val outerRadius = 150f
    private val innerRadius = 60f

    private var IsPressed = false
    private var joystickPosition = PointF()

    init {

    }

    override fun onMeasure(widthMeasureSpec: Int, heightMeasureSpec: Int) {
        val width = (outerRadius * 10).toInt()
        val height = (outerRadius * 10).toInt()
        setMeasuredDimension(width, height)
    }

    override fun onDraw(canvas: Canvas) {
        /*val centerX = (width / 2).toFloat()
        val centerY = (height / 2).toFloat()
        joystickPosition.set(centerX, centerY)*/
        canvas.drawCircle(center.x, center.y, outerRadius, paint)
        canvas.drawCircle(joystickPosition.x, joystickPosition.y, innerRadius, paint)
    }

    override fun onTouchEvent(event: MotionEvent): Boolean {
        when (event.action) {
            MotionEvent.ACTION_DOWN -> {
                isPressed = true
                updateJoystickPosition(event.x, event.y)
            }
            MotionEvent.ACTION_MOVE -> {
                updateJoystickPosition(event.x, event.y)
            }
            MotionEvent.ACTION_UP, MotionEvent.ACTION_CANCEL -> {
                isPressed = false
                resetJoystickPosition()
            }
        }
        return true
    }

    private fun updateJoystickPosition(x: Float, y: Float) {
        val dx = x - center.x
        val dy = y - center.y
        val distance = Math.sqrt((dx * dx).toDouble() + (dy * dy).toDouble())
        if (distance > outerRadius) {
            joystickPosition.x = center.x + outerRadius * dx / distance.toFloat()
            joystickPosition.y = center.y + outerRadius * dy / distance.toFloat()
        } else {
            joystickPosition.x = x
            joystickPosition.y = y
        }
        invalidate()
    }

    private fun resetJoystickPosition() {
        joystickPosition.x = center.x
        joystickPosition.y = center.y
        invalidate()
    }
}