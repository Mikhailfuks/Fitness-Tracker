import UIKit
import CoreMotion

class FitnessTrackerViewController: UIViewController {

  // UI elements
  @IBOutlet weak var stepsLabel: UILabel!
  @IBOutlet weak var distanceLabel: UILabel!
  @IBOutlet weak var caloriesLabel: UILabel!
  
  // CoreMotion objects
  let motionManager = CMMotionActivityManager()
  let pedometer = CMPedometer()

  // Data variables
  var totalSteps: Int = 0
  var distance: Double = 0.0
  var caloriesBurned: Double = 0.0

  override func viewDidLoad() {
    super.viewDidLoad()
    
    // Request authorization for motion data
    requestMotionAuthorization()
    
    // Start tracking steps
    startTrackingSteps()
  }

  // Request authorization for motion data
  func requestMotionAuthorization() {
    if CMMotionActivityManager.isActivityAvailable() {
      motionManager.startActivityUpdates(to: .main) { activity in
        if let activity = activity {
          // Update UI based on activity data
   
        }
      }
    }
  }

  // Start tracking steps
  func startTrackingSteps() {
    if CMPedometer.isStepCountingAvailable() {
      pedometer.startUpdates(from: Date()) { (data, error) in
        if let data = data {
          self.totalSteps = Int(data.numberOfSteps)
          self.updateStepsLabel()
          
          // Calculate distance and calories (optional)
          // ...
        } else if let error = error {
          print("Error tracking steps: (error)")
        }
      }
    } else {
      print("Step counting is not available.")
    }
  }

  // Update UI elements
  func updateStepsLabel() {
    stepsLabel.text = "Steps: (totalSteps)"
  }

  func updateDistanceLabel() {
    distanceLabel.text = "Distance: (distance) km"
  }

  func updateCaloriesLabel() {
    caloriesLabel.text = "Calories: (caloriesBurned)"
  }
}
