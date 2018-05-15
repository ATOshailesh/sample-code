# Sample-code
For get information.

## Convert server date to Display Date
	struct DateFormet {
		static let serverDateFormate = "yyyy-MM-dd HH:mm:ss"
		static let dateFormet 	     = "yyyy-MM-dd"
  	}
	
 	 func convertServerDateToDisplayDateFormet() -> String {
		let dateformet = DateFormatter()
    		dateformet.dateFormat = DateFormet.serverDateFormate
    		if(dateformet.date(from: self) != nil){
			let date = dateformet.date(from: self)
        		dateformet.dateFormat = DateFormet.dateFormet
        		return dateformet.string(from: date!)
     	}
     	return self
 	 }
	 
## Get time ago
	func getTimesAgo(_ date : Date) -> String {
        
        let units = Set<Calendar.Component>([.year, .month, .day, .weekOfYear])
        let components = Calendar.current.dateComponents(units, from: date as Date, to: Date())
        
        if components.year! > 0 {
            return ("\(components.year!)y ago")
            
        } else if components.month! > 0 {
            return ("\(components.month!)m ago")
            
        } else if components.weekOfYear! > 0 {
            return ("\(components.weekOfYear!)w ago")
            
        } else if (components.day! > 0) {
            return ("\(components.day!)d ago")
            
        } else {
            return "To day"
        }
	}


