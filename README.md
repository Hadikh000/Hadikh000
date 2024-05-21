dependencies {
    implementation "androidx.work:work-runtime-ktx:2.7.1" // Use the latest version
}
import android.content.Context
import androidx.work.Worker
import androidx.work.WorkerParameters

class MyWorker(appContext: Context, workerParams: WorkerParameters) : Worker(appContext, workerParams) {
    override fun doWork(): Result {
        // Do the work here

        // Indicate whether the work finished successfully with the Result
        return Result.success()
    }
}
import androidx.appcompat.app.AppCompatActivity
import android.os.Bundle
import androidx.work.OneTimeWorkRequest
import androidx.work.WorkManager

class MainActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        val workRequest = OneTimeWorkRequest.Builder(MyWorker::class.java).build()
        WorkManager.getInstance(this).enqueue(workRequest)
    }
}
import androidx.work.Data

val inputData = Data.Builder()
    .putString("key", "value")
    .build()

val workRequest = OneTimeWorkRequest.Builder(MyWorker::class.java)
    .setInputData(inputData)
    .build()
override fun doWork(): Result {
    val value = inputData.getString("key")
    // Use the value
    return Result.success()
}
import androidx.work.Data

class MyWorker(appContext: Context, workerParams: WorkerParameters) : Worker(appContext, workerParams) {
    override fun doWork(): Result {
        // Do the work here

        val outputData = Data.Builder()
            .putString("outputKey", "outputValue")
            .build()

        return Result.success(outputData)
    }
}
import androidx.lifecycle.Observer
import androidx.work.WorkInfo

WorkManager.getInstance(this)
    .getWorkInfoByIdLiveData(workRequest.id)
    .observe(this, Observer { workInfo ->
        if (workInfo != null && workInfo.state.isFinished) {
            val outputData = workInfo.outputData
            val outputValue = outputData.getString("outputKey")
            // Use the output value
        }
    })
