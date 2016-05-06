



### Adding multiple columns to a DataFrame

This snippet demonstrates how to use foldLeft to chain the
addition of multiple columns.
```scala
import org.apache.spark.sql.DataFrame
import org.apache.spark.sql.functions._

// Create DataFrame
val foodCounts = sqlContext.sparkContext.parallelize(List((1,1,1,1),(2,4,8,16),(3,9,27,81))).toDF("hamburgers", "hotdogs", "chips", "onion dip")

// Compute percentage of row for each column
val foods = foodCounts.columns
val rowTotal = foods.map(col(_)).reduce(_ + _)
// Create name-column pairs
val percentColumns = foods.map{ food =>
    (s"${food}Pct", col(food) / $"total") 

// The fold left initializes the accumulator with the foodCounts dataframe
// with a total column. The foldLeft has repeated withColumns
percentColumns.
    foldLeft(foodCounts.withColumn("total", rowTotal))
    ((b, a) => b.withColumn(a._1, a._2)).show

}
```




