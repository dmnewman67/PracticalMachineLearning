Get Data
--------

Data

The training data for this project are available here:

<https://d396qusza40orc.cloudfront.net/predmachlearn/pml-training.csv>

The test data are available here:

<https://d396qusza40orc.cloudfront.net/predmachlearn/pml-testing.csv>

The data for this project come from this source:
<http://web.archive.org/web/20161224072740/http:/groupware.les.inf.puc-rio.br/har>.
If you use the document you create for this class for any purpose please
cite them as they have been very generous in allowing their data to be
used for this kind of assignmen

> Using devices such as Jawbone Up, Nike FuelBand, and Fitbit it is now
> possible to collect a large amount of data about personal activity
> relatively inexpensively. These type of devices are part of the
> quantified self movement - a group of enthusiasts who take
> measurements about themselves regularly to improve their health, to
> find patterns in their behavior, or because they are tech geeks. One
> thing that people regularly do is quantify how much of a particular
> activity they do, but they rarely quantify how well they do it. In
> this project, your goal will be to use data from accelerometers on the
> belt, forearm, arm, and dumbell of 6 participants. They were asked to
> perform barbell lifts correctly and incorrectly in 5 different ways.
> More information is available from the website here:
> <http://web.archive.org/web/20161224072740/http:/groupware.les.inf.puc-rio.br/har>
> (see the section on the Weight Lifting Exercise Dataset).

Load the data

    training <- read.csv("pml-training.csv")
    testing <- read.csv("pml-testing.csv")

The Goal
--------

> The goal of your project is to predict the manner in which they did
> the exercise. This is the "classe" variable in the training set. You
> may use any of the other variables to predict with. You should create
> a report describing how you built your modelel, how you used cross
> validation, what you think the expected out of sample error is, and
> why you made the choices you did. You will also use your prediction
> modelel to predict 20 different test cases. Data Summary:

    levels(training$user_name)
    levels(training$classe)
    training <- read.csv("pml-training.csv", na.strings=c("NA","#DIV/0!", ""))
    training <- training[, colSums(is.na(training)) == 0]
    testing <- read.csv("pml-testing.csv", na.strings=c("NA","#DIV/0!", ""))
    testing <- testing[, colSums(is.na(testing)) == 0]

Model the data
--------------

Create some random forest models:

Belt:

    modelBelt_rf <- train(classe ~ roll_belt + pitch_belt + yaw_belt, method = "rf", data = training, ntree = 20)
    Belt_rf <- predict(modelBelt_rf, training)
    sum(training$classe == Belt_rf) / length(Belt_rf)

Forearm:

    modelForearm_rf <- train(classe ~ roll_forearm + pitch_forearm + yaw_forearm, method = "rf", data = training, ntree = 20)
    Forearm_rf <- predict(modelForearm_rf, training)
    sum(training$classe == Forearm_rf) / length(Forearm_rf)

Arm:

    modelArm_rf <- train(classe ~ roll_arm + pitch_arm + yaw_arm, method = "rf", data = training, ntree = 20)
    Arm_rf <- predict(modelArm_rf, training)
    sum(training$classe == Arm_rf) / length(Arm_rf)

Test Data
---------

Apply the models to the Testing data set:

    BeltTest <- predict(modelBelt_rf, testing)
    ForearmTest <- predict(modelForearm_rf, testing)
    ArmTest <- predict(modelArm_rf, testing)
    table(BeltTest, ForearmTest, ArmTest)
