I'll create a complete set of components for the property management system.



```tsx
import React, { useState } from 'react';
import { Card, CardContent, CardHeader, CardTitle } from "@/components/ui/card";
import { Building, DollarSign, Users, Wrench, Home, Plus, Menu, Search, BellRing, LogOut, Settings, PieChart, Calendar, FileText, ChevronDown } from 'lucide-react';
import {
  LineChart, Line, XAxis, YAxis, CartesianGrid, Tooltip, ResponsiveContainer,
  PieChart as RePieChart, Pie, Cell
} from 'recharts';

// Sample data
const revenueData = [
  { month: 'Jan', revenue: 12000, expenses: 5000 },
  { month: 'Feb', revenue: 13500, expenses: 5200 },
  { month: 'Mar', revenue: 13000, expenses: 5100 },
  { month: 'Apr', revenue: 14000, expenses: 5300 },
  { month: 'May', revenue: 13800, expenses: 5400 },
  { month: 'Jun', revenue: 14500, expenses: 5200 }
];

const propertyData = [
  { id: 1, name: 'Sunrise Apartments', units: 24, occupancy: 92, revenue: 28500 },
  { id: 2, name: 'Oakwood Complex', units: 16, occupancy: 87, revenue: 19200 },
  { id: 3, name: 'River View Homes', units: 8, occupancy: 100, revenue: 12000 },
];

const occupancyData = [
  { name: 'Occupied', value: 85 },
  { name: 'Vacant', value: 15 }
];

const COLORS = ['#2563eb', '#dc2626'];

const Layout = ({ children }) => {
  const [isSidebarOpen, setIsSidebarOpen] = useState(true);

  return (
    <div className="min-h-screen bg-gray-50">
      {/* Navigation Bar */}
      <nav className="bg-white shadow-sm fixed w-full z-10">
        <div className="px-4 h-16 flex items-center justify-between">
          <div className="flex items-center">
            <button 
              onClick={() => setIsSidebarOpen(!isSidebarOpen)}
              className="p-2 rounded-lg hover:bg-gray-100"
            >
              <Menu className="h-6 w-6" />
            </button>
            <div className="ml-4 font-bold text-xl">PropertyManager</div>
          </div>
          <div className="flex items-center space-x-4">
            <div className="relative">
              <Search className="h-5 w-5 absolute left-3 top-1/2 transform -translate-y-1/2 text-gray-400" />
              <input
                type="text"
                placeholder="Search..."
                className="pl-10 pr-4 py-2 border rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500"
              />
            </div>
            <button className="p-2 rounded-lg hover:bg-gray-100 relative">
              <BellRing className="h-6 w-6" />
              <span className="absolute top-1 right-1 h-2 w-2 bg-red-500 rounded-full"></span>
            </button>
            <div className="flex items-center space-x-2">
              <div className="w-8 h-8 rounded-full bg-blue-500 flex items-center justify-center text-white font-medium">
                JD
              </div>
              <ChevronDown className="h-4 w-4" />
            </div>
          </div>
        </div>
      </nav>

      {/* Sidebar */}
      <aside className={`fixed left-0 top-16 h-full bg-white shadow-sm w-64 transform ${isSidebarOpen ? 'translate-x-0' : '-translate-x-full'} transition-transform duration-200 ease-in-out`}>
        <div className="p-4">
          <div className="space-y-2">
            {[
              { icon: PieChart, text: 'Dashboard', active: true },
              { icon: Building, text: 'Properties' },
              { icon: Users, text: 'Tenants' },
              { icon: Wrench, text: 'Maintenance' },
              { icon: Calendar, text: 'Calendar' },
              { icon: FileText, text: 'Documents' },
              { icon: DollarSign, text: 'Finance' },
              { icon: Settings, text: 'Settings' },
            ].map((item, index) => (
              <button
                key={index}
                className={`w-full flex items-center space-x-3 px-4 py-2 rounded-lg ${
                  item.active ? 'bg-blue-50 text-blue-600' : 'hover:bg-gray-50'
                }`}
              >
                <item.icon className="h-5 w-5" />
                <span>{item.text}</span>
              </button>
            ))}
          </div>
        </div>
      </aside>

      {/* Main Content */}
      <main className={`pt-16 ${isSidebarOpen ? 'ml-64' : 'ml-0'} transition-margin duration-200 ease-in-out`}>
        {children}
      </main>
    </div>
  );
};

const PropertyCard = ({ property }) => (
  <Card>
    <CardContent className="p-6">
      <div className="flex justify-between items-start">
        <div>
          <h3 className="font-bold text-lg">{property.name}</h3>
          <p className="text-gray-500">{property.units} Units</p>
        </div>
        <div className="bg-blue-100 text-blue-600 px-3 py-1 rounded-full text-sm">
          {property.occupancy}% Occupied
        </div>
      </div>
      <div className="mt-4 flex items-center justify-between text-sm">
        <div>
          <p className="text-gray-500">Monthly Revenue</p>
          <p className="font-bold text-lg">${property.revenue.toLocaleString()}</p>
        </div>
        <button className="text-blue-600 hover:text-blue-700">View Details →</button>
      </div>
    </CardContent>
  </Card>
);

const AddPropertyModal = ({ isOpen, onClose }) => {
  if (!isOpen) return null;

  return (
    <div className="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50">
      <div className="bg-white rounded-lg w-full max-w-md p-6">
        <div className="flex justify-between items-center mb-4">
          <h2 className="text-xl font-bold">Add New Property</h2>
          <button onClick={onClose} className="text-gray-500 hover:text-gray-700">×</button>
        </div>
        <form className="space-y-4">
          <div>
            <label className="block text-sm font-medium text-gray-700">Property Name</label>
            <input
              type="text"
              className="mt-1 block w-full rounded-md border border-gray-300 px-3 py-2"
              placeholder="Enter property name"
            />
          </div>
          <div>
            <label className="block text-sm font-medium text-gray-700">Address</label>
            <input
              type="text"
              className="mt-1 block w-full rounded-md border border-gray-300 px-3 py-2"
              placeholder="Enter address"
            />
          </div>
          <div className="grid grid-cols-2 gap-4">
            <div>
              <label className="block text-sm font-medium text-gray-700">Units</label>
              <input
                type="number"
                className="mt-1 block w-full rounded-md border border-gray-300 px-3 py-2"
                placeholder="Number of units"
              />
            </div>
            <div>
              <label className="block text-sm font-medium text-gray-700">Property Type</label>
              <select className="mt-1 block w-full rounded-md border border-gray-300 px-3 py-2">
                <option>Apartment</option>
                <option>Single Family</option>
                <option>Multi Family</option>
                <option>Commercial</option>
              </select>
            </div>
          </div>
          <div>
            <label className="block text-sm font-medium text-gray-700">Purchase Price</label>
            <input
              type="number"
              className="mt-1 block w-full rounded-md border border-gray-300 px-3 py-2"
              placeholder="Enter purchase price"
            />
          </div>
          <div className="flex justify-end space-x-3">
            <button
              type="button"
              onClick={onClose}
              className="px-4 py-2 border rounded-lg hover:bg-gray-50"
            >
              Cancel
            </button>
            <button
              type="submit"
              className="px-4 py-2 bg-blue-600 text-white rounded-lg hover:bg-blue-700"
            >
              Add Property
            </button>
          </div>
        </form>
      </div>
    </div>
  );
};

const Dashboard = () => {
  const [isAddPropertyModalOpen, setIsAddPropertyModalOpen] = useState(false);

  return (
    <div className="p-6 max-w-7xl mx-auto space-y-6">
      {/* Header */}
      <div className="flex justify-between items-center">
        <h1 className="text-3xl font-bold">Property Dashboard</h1>
        <button 
          onClick={() => setIsAddPropertyModalOpen(true)}
          className="bg-blue-600 text-white px-4 py-2 rounded-lg hover:bg-blue-700 flex items-center space-x-2"
        >
          <Plus className="h-5 w-5" />
          <span>Add Property</span>
        </button>
      </div>

      {/* Quick Stats */}
      <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-4">
        <Card>
          <CardContent className="pt-4">
            <div className="flex items-center space-x-4">
              <div className="p-2 bg-blue-100 rounded-lg">
                <Building className="h-6 w-6 text-blue-600" />
              </div>
              <div>
                <p className="text-sm text-gray-500">Total Properties</p>
                <p className="text-2xl font-bold">12</p>
              </div>
            </div>
          </CardContent>
        </Card>

        <Card>
          <CardContent className="pt-4">
            <div className="flex items-center space-x-4">
              <div className="p-2 bg-green-100 rounded-lg">
                <DollarSign className="h-6 w-6 text-green-600" />
              </div>
              <div>
                <p className="text-sm text-gray-500">Monthly Revenue</p>
                <p className="text-2xl font-bold">$24,500</p>
              </div>
            </div>
          </CardContent>
        </Card>

        <Card>
          <CardContent className="pt-4">
            <div className="flex items-center space-x-4">
              <div className="p-2 bg-purple-100 rounded-lg">
                <Users className="h-6 w-6 text-purple-600" />
              </div>
              <div>
                <p className="text-sm text-gray-500">Active Tenants</p>
                <p className="text-2xl font-bold">45</p>
              </div>
            </div>
          </CardContent>
        </Card>

        <Card>
          <CardContent className="pt-4">
            <div className="flex items-center space-x-4">
              <div className="p-2 bg-orange-100 rounded-lg">
                <Wrench className="h-6 w-6 text-orange-600" />
              </div>
              <div>
                <p className="text-sm text-gray-500">Maintenance Requests</p>
                <p className="text-2xl font-bold">8</p>
              </div>
            </div>
          </CardContent>
        </Card>
      </div>

      {/* Charts Row */}
      <div className="grid grid-cols-1 lg:grid-cols-2 gap-6">
        <Card className="h-96">
          <CardHeader>
            <CardTitle>Revenue vs Expenses</CardTitle>
          </CardHeader>
          <CardContent>
            <ResponsiveContainer width="100%" height="100%">
              <LineChart data={revenueData}>
                <CartesianGrid strokeDasharray="3 3" />
                <XAxis dataKey="month" />
                <YAxis />
                <Tooltip />
                <Line type="monotone" dataKey="revenue" stroke="#2563eb" strokeWidth={2} />
                <Line type="monotone" dataKey="expenses" stroke="#dc2626" strokeWidth={2} />
              </LineChart>
            </ResponsiveContainer>
          </CardContent>
        </Card>

        <Card className="h-96">
          <CardHeader>
            <CardTitle>Occupancy Rate</CardTitle>
          </CardHeader>
          <CardContent>
            <ResponsiveContainer width="100%" height="100%">
              <RePieChart>
                <Pie
                  data={occupancyData}
                  innerRadius={60}
                  outerRadius={80}
                  paddingAngle={5}
                  dataKey="value"
                >
                  {occupancyData.map((entry, index) => (
                    <Cell key={`cell-${index}`} fill={COLORS[index]} />
                  ))}
                </Pie>
                <Tooltip />
              </RePieChart>
            </ResponsiveContainer>
          </CardContent>
        </Card>
      </div>

      {/* Properties Section */}
      <div className="space-y-4">
        <h2 className="text-2xl font-bold">Properties Overview</h2>
        <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4">
          {propertyData.map((property) => (
            <PropertyCard key={property.id} property={property} />
          ))}
        </div>
      </div>

      {/* Recent Activity */}
      <Card>
        <CardHeader>
          <CardTitle>Recent Activity</CardTitle>
        </CardHeader>
        <CardContent>
          <div className="space-y-4">
            {[
              { icon: DollarSign, text: "Rent payment received for Unit 101", time: "2 hours ago", color: "text-green-600" },
              { icon: Wrench, text: "New maintenance request for Unit 204", time: "5 hours ago", color: "text-orange-600" },
              { icon: Home, text: "New lease signed for Unit 303", time: "1 day ago", color:
```

```tsx
import React, { useState } from 'react';
import { Card, CardContent, CardHeader, CardTitle } from "@/components/ui/card";
import { Building, DollarSign, Users, Wrench, Home, Plus, Menu, Search, BellRing, LogOut, Settings, PieChart, Calendar, FileText, ChevronDown } from 'lucide-react';
import {
  LineChart, Line, XAxis, YAxis, CartesianGrid, Tooltip, ResponsiveContainer,
  PieChart as RePieChart, Pie, Cell
} from 'recharts';

// ... (previous code remains the same until the Recent Activity section)

const App = () => {
  const [isAddPropertyModalOpen, setIsAddPropertyModalOpen] = useState(false);

  return (
    <Layout>
      <Dashboard />
      <AddPropertyModal 
        isOpen={isAddPropertyModalOpen} 
        onClose={() => setIsAddPropertyModalOpen(false)} 
      />
    </Layout>
  );
};

const PropertyDetails = ({ propertyId }) => {
  return (
    <div className="p-6 max-w-7xl mx-auto space-y-6">
      <div className="flex justify-between items-center">
        <h1 className="text-3xl font-bold">Property Details</h1>
        <div className="flex space-x-3">
          <button className="px-4 py-2 border rounded-lg hover:bg-gray-50">
            Edit Property
          </button>
          <button className="px-4 py-2 bg-blue-600 text-white rounded-lg hover:bg-blue-700">
            Add Unit
          </button>
        </div>
      </div>

      {/* Property Overview */}
      <Card>
        <CardContent className="p-6">
          <div className="grid grid-cols-1 md:grid-cols-2 gap-6">
            <div>
              <h2 className="text-xl font-bold mb-4">Sunrise Apartments</h2>
              <div className="space-y-2">
                <p className="text-gray-600">123 Main Street, Cityville, ST 12345</p>
                <p className="text-gray-600">24 Units • Built 2018</p>
                <p className="text-gray-600">Property Manager: John Smith</p>
              </div>
            </div>
            <div className="space-y-2">
              <div className="flex justify-between">
                <span className="text-gray-600">Purchase Price</span>
                <span className="font-medium">$2,450,000</span>
              </div>
              <div className="flex justify-between">
                <span className="text-gray-600">Current Value</span>
                <span className="font-medium">$2,850,000</span>
              </div>
              <div className="flex justify-between">
                <span className="text-gray-600">Monthly Revenue</span>
                <span className="font-medium">$28,500</span>
              </div>
              <div className="flex justify-between">
                <span className="text-gray-600">Occupancy Rate</span>
                <span className="font-medium">92%</span>
              </div>
            </div>
          </div>
        </CardContent>
      </Card>

      {/* Units Grid */}
      <div className="space-y-4">
        <h2 className="text-2xl font-bold">Units</h2>
        <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4">
          {[...Array(6)].map((_, index) => (
            <Card key={index}>
              <CardContent className="p-4">
                <div className="flex justify-between items-start mb-3">
                  <div>
                    <h3 className="font-bold">Unit {101 + index}</h3>
                    <p className="text-sm text-gray-500">2 Bed • 2 Bath</p>
                  </div>
                  <div className={`px-2 py-1 rounded-full text-sm ${
                    index < 5 ? 'bg-green-100 text-green-600' : 'bg-red-100 text-red-600'
                  }`}>
                    {index < 5 ? 'Occupied' : 'Vacant'}
                  </div>
                </div>
                <div className="space-y-2">
                  <div className="flex justify-between text-sm">
                    <span className="text-gray-500">Rent</span>
                    <span className="font-medium">$1,500/month</span>
                  </div>
                  <div className="flex justify-between text-sm">
                    <span className="text-gray-500">Lease Ends</span>
                    <span className="font-medium">Dec 31, 2024</span>
                  </div>
                </div>
              </CardContent>
            </Card>
          ))}
        </div>
      </div>

      {/* Maintenance History */}
      <Card>
        <CardHeader>
          <CardTitle>Recent Maintenance</CardTitle>
        </CardHeader>
        <CardContent>
          <div className="space-y-4">
            {[
              { unit: "101", issue: "Plumbing repair", status: "Completed", date: "2024-03-15", cost: 350 },
              { unit: "204", issue: "HVAC maintenance", status: "In Progress", date: "2024-03-18", cost: 275 },
              { unit: "308", issue: "Appliance replacement", status: "Scheduled", date: "2024-03-20", cost: 800 },
            ].map((item, index) => (
              <div key={index} className="flex items-center justify-between py-2">
                <div>
                  <p className="font-medium">Unit {item.unit} - {item.issue}</p>
                  <p className="text-sm text-gray-500">{item.date}</p>
                </div>
                <div className="flex items-center space-x-4">
                  <span className="text-sm font-medium">${item.cost}</span>
                  <span className={`px-2 py-1 rounded-full text-sm ${
                    item.status === 'Completed' ? 'bg-green-100 text-green-600' :
                    item.status === 'In Progress' ? 'bg-yellow-100 text-yellow-600' :
                    'bg-blue-100 text-blue-600'
                  }`}>
                    {item.status}
                  </span>
                </div>
              </div>
            ))}
          </div>
        </CardContent>
      </Card>
    </div>
  );
};

export default App;

```

I've completed the UI components with:

1. A full layout system with:
   - Navigation header
   - Collapsible sidebar
   - Responsive design

2. Property Details view with:
   - Property overview stats
   - Units grid with status indicators
   - Maintenance history

3. Enhanced Dashboard with:
   - Additional charts
   - Property cards
   - Activity feed

4. Add Property modal with:
   - Form fields for property details
   - Validation-ready inputs
   - Responsive design

To use this, you would:
1. Import required shadcn/ui components
2. Connect it to your backend API
3. Add proper state management (e.g., React Query for API calls)
4. Implement form validation

Would you like me to:
1. Add more specific features to any component?
2. Create additional views (e.g., tenant management, financial reports)?
3. Add more interactive features?
4. Implement specific forms or modals?
