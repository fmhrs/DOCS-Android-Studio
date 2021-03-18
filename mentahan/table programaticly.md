package com.example.smartwarninglight.mac

import androidx.appcompat.app.AppCompatActivity
import android.os.Bundle
import android.view.Gravity
import android.widget.TableRow
import android.widget.TextView
import android.widget.Toast
import com.example.smartwarninglight.databinding.ActivityChartBinding
import com.example.smartwarninglight.model.dataData

class ChartActivity : AppCompatActivity() {


    lateinit var idMac: String
    lateinit var nameMac: String

    lateinit var binding: ActivityChartBinding
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        binding = ActivityChartBinding.inflate(layoutInflater)
        setContentView(binding.root)

        intent.getStringExtra("ID_MAC")?.let { idMac = it }
        intent.getStringExtra("NAME_MAC")?.let { nameMac = it }

        binding.textTitle.text = "Chart $idMac"

        val mutableData = mutableListOf(
            dataData(1, "CC55T", 35f, 6f, 4f, "2021-03-18 13:49:32" ),
            dataData(2, "CC55T", 36f, 8f, 10f, "2021-03-16 16:44:46" ),
            dataData(3, "CC55T", 35f, 8f, 10f, "2021-03-16 16:44:48" ),
            dataData(4, "CC55T", 35f, 4f, 9f, "2021-03-16 16:44:51" ),
            dataData(5, "CC56T", 37f, 10f, 9f, "2021-03-16 13:50:53" ),
            dataData(6, "CC58T", 35f, 10f, 9f, "2021-03-16 16:44:53" ),
            dataData(7, "CC57T", 38f, 10f, 9f, "2021-03-16 16:44:56" ),
            dataData(8, "CC57T", 36f, 11f, 9f, "2021-03-16 16:44:58" ),
            dataData(9, "CC57T", 36f, 8f, 9f, "2021-03-16 16:45:00" ),
            dataData(10, "CC56T", 36f, 8f, 7f, "2021-03-16 13:51:29" )
        )

        Toast.makeText(this, "$mutableData", Toast.LENGTH_SHORT).show()


//        val adapter = DataRecyclerAdapter(mutableData, this)
//        binding.recyclerView.layoutManager = LinearLayoutManager(this)
//        binding.recyclerView.setHasFixedSize(true)
//        binding.recyclerView.adapter = adapter



        binding.tableLayout.setColumnStretchable(0, true)
        binding.tableLayout.setColumnStretchable(1, true)

        tableLayoutAdapter(mutableData)
    }

    private fun tableLayoutAdapter(mutabledData: MutableList<dataData>){
        mutabledData.forEach {
            binding.tableLayout.addView(
                makeTableRow(it)
            )
        }
    }

    private fun makeTableRow(data: dataData): TableRow {
        return TableRow(this).apply {
            this.addView(makeTextView("${data.ID_DATA}"))
            this.addView(makeTextView("${data.ID_MAC}"))
            this.addView(makeTextView("${data.SUHU}"))
            this.addView(makeTextView("${data.ARUS}"))
            this.addView(makeTextView("${data.WAKTU}"))
        }
    }

    private fun makeTextView(text: String): TextView{
        return TextView(this).apply {
            this.setText("$text")
            this.gravity=Gravity.CENTER
        }
    }
}
