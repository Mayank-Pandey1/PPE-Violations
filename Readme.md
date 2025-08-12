Basic next.js app
```javascript
// pages/index.js
import React, { useState } from 'react';
import { LineChart, Line, XAxis, YAxis, CartesianGrid, Tooltip, ResponsiveContainer, PieChart, Pie, Cell } from 'recharts';

// Sample data
const incidentTrends = [
  { date: '2024-01-01', ppeViolations: 15, fallIncidents: 3, accessBreaches: 2 },
  { date: '2024-01-02', ppeViolations: 18, fallIncidents: 2, accessBreaches: 1 },
  { date: '2024-01-03', ppeViolations: 12, fallIncidents: 4, accessBreaches: 3 },
  { date: '2024-01-04', ppeViolations: 20, fallIncidents: 1, accessBreaches: 2 },
  { date: '2024-01-05', ppeViolations: 16, fallIncidents: 3, accessBreaches: 1 },
  { date: '2024-01-06', ppeViolations: 14, fallIncidents: 2, accessBreaches: 0 },
  { date: '2024-01-07', ppeViolations: 19, fallIncidents: 5, accessBreaches: 2 },
];

const severityBreakdown = [
  { name: 'High', value: 25, color: '#ef4444' },
  { name: 'Medium', value: 45, color: '#f59e0b' },
  { name: 'Low', value: 30, color: '#10b981' },
];

// Components
const MetricCard = ({ title, value, change, changeType, period }) => {
  const changeColor = changeType === 'increase' ? 'text-red-500' : 'text-green-500';
  const changeIcon = changeType === 'increase' ? '↑' : '↓';

  return (
    <div className="bg-white p-6 rounded-lg shadow-sm border border-gray-200">
      <h3 className="text-sm font-medium text-gray-500 mb-2">{title}</h3>
      <div className="flex items-end justify-between">
        <span className="text-3xl font-bold text-gray-900">{value}</span>
        <div className="text-sm">
          <span className={`${changeColor} font-medium`}>
            {changeIcon} {change}
          </span>
          <span className="text-gray-500 ml-1">{period}</span>
        </div>
      </div>
    </div>
  );
};

const Dashboard = () => {
  const [timeFilter, setTimeFilter] = useState('All Time');
  const [siteFilter, setSiteFilter] = useState('All Sites');

  return (
    <div className="min-h-screen bg-gray-50">
      {/* Header */}
      <header className="bg-white border-b border-gray-200 px-6 py-4">
        <div className="flex items-center justify-between">
          <h1 className="text-2xl font-bold text-gray-900">Dashboard</h1>
          <div className="flex items-center space-x-4">
            <select
              value={timeFilter}
              onChange={(e) => setTimeFilter(e.target.value)}
              className="px-3 py-2 border border-gray-300 rounded-md text-sm focus:outline-none focus:ring-2 focus:ring-blue-500"
            >
              <option>All Time</option>
              <option>Last 30 Days</option>
              <option>Last 7 Days</option>
              <option>Today</option>
            </select>
            <select
              value={siteFilter}
              onChange={(e) => setSiteFilter(e.target.value)}
              className="px-3 py-2 border border-gray-300 rounded-md text-sm focus:outline-none focus:ring-2 focus:ring-blue-500"
            >
              <option>All Sites</option>
              <option>Site A</option>
              <option>Site B</option>
              <option>Site C</option>
            </select>
            <button className="px-4 py-2 bg-blue-600 text-white rounded-md hover:bg-blue-700 text-sm font-medium">
              Filters
            </button>
            <button className="px-4 py-2 bg-gray-600 text-white rounded-md hover:bg-gray-700 text-sm font-medium">
              Refresh
            </button>
          </div>
        </div>
      </header>

      <div className="px-6 py-6">
        {/* Category Tabs */}
        <div className="flex space-x-8 mb-8 border-b border-gray-200">
          {['Overall', 'PPE Violations', 'Fall Incidents', 'Access Breach', 'Staff Monitoring'].map((tab) => (
            <button
              key={tab}
              className={`pb-3 px-1 border-b-2 font-medium text-sm ${
                tab === 'Overall'
                  ? 'border-blue-500 text-blue-600'
                  : 'border-transparent text-gray-500 hover:text-gray-700 hover:border-gray-300'
              }`}
            >
              {tab}
            </button>
          ))}
        </div>

        {/* Main Metrics Grid */}
        <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6 mb-8">
          <MetricCard
            title="Total PPE Violations"
            value="100"
            change="15%"
            changeType="increase"
            period="last week"
          />
          <MetricCard
            title="Days Since Last Incident"
            value="5"
            change="10%"
            changeType="decrease"
            period="last week"
          />
          <MetricCard
            title="Incident Compliance Rate"
            value="89.4%"
            change="10%"
            changeType="decrease"
            period="last week"
          />
          <MetricCard
            title="Workforce Onsite"
            value="75"
            change="10%"
            changeType="decrease"
            period="last week"
          />
        </div>

        {/* Secondary Metrics Grid */}
        <div className="grid grid-cols-1 md:grid-cols-3 gap-6 mb-8">
          <MetricCard
            title="PPE Violations"
            value="23"
            change="10%"
            changeType="decrease"
            period="last week"
          />
          <MetricCard
            title="Access Breaches"
            value="4"
            change="10%"
            changeType="decrease"
            period="last week"
          />
          <MetricCard
            title="Fall Incidents"
            value="7"
            change="10%"
            changeType="decrease"
            period="last week"
          />
        </div>

        {/* Charts Section */}
        <div className="grid grid-cols-1 lg:grid-cols-2 gap-8">
          {/* Incident Trends Chart */}
          <div className="bg-white p-6 rounded-lg shadow-sm border border-gray-200">
            <h3 className="text-lg font-semibold text-gray-900 mb-4">Incident Trends</h3>
            <ResponsiveContainer width="100%" height={300}>
              <LineChart data={incidentTrends}>
                <CartesianGrid strokeDasharray="3 3" stroke="#f1f5f9" />
                <XAxis 
                  dataKey="date" 
                  stroke="#64748b"
                  tick={{ fontSize: 12 }}
                />
                <YAxis stroke="#64748b" tick={{ fontSize: 12 }} />
                <Tooltip 
                  contentStyle={{ 
                    backgroundColor: '#ffffff',
                    border: '1px solid #e2e8f0',
                    borderRadius: '8px'
                  }}
                />
                <Line 
                  type="monotone" 
                  dataKey="ppeViolations" 
                  stroke="#ef4444" 
                  strokeWidth={2}
                  name="PPE Violations"
                />
                <Line 
                  type="monotone" 
                  dataKey="fallIncidents" 
                  stroke="#f59e0b" 
                  strokeWidth={2}
                  name="Fall Incidents"
                />
                <Line 
                  type="monotone" 
                  dataKey="accessBreaches" 
                  stroke="#3b82f6" 
                  strokeWidth={2}
                  name="Access Breaches"
                />
              </LineChart>
            </ResponsiveContainer>
          </div>

          {/* Incident Severity Breakdown Chart */}
          <div className="bg-white p-6 rounded-lg shadow-sm border border-gray-200">
            <h3 className="text-lg font-semibold text-gray-900 mb-4">Incident Severity Breakdown</h3>
            <ResponsiveContainer width="100%" height={300}>
              <PieChart>
                <Pie
                  data={severityBreakdown}
                  cx="50%"
                  cy="50%"
                  outerRadius={100}
                  dataKey="value"
                  label={({ name, percent }) => `${name} ${(percent * 100).toFixed(0)}%`}
                >
                  {severityBreakdown.map((entry, index) => (
                    <Cell key={`cell-${index}`} fill={entry.color} />
                  ))}
                </Pie>
                <Tooltip />
              </PieChart>
            </ResponsiveContainer>
            <div className="flex justify-center space-x-6 mt-4">
              {severityBreakdown.map((item) => (
                <div key={item.name} className="flex items-center">
                  <div 
                    className="w-3 h-3 rounded-full mr-2" 
                    style={{ backgroundColor: item.color }}
                  ></div>
                  <span className="text-sm text-gray-600">{item.name}</span>
                </div>
              ))}
            </div>
          </div>
        </div>
      </div>
    </div>
  );
};

export default Dashboard;

// package.json dependencies needed:
/*
{
  "dependencies": {
    "next": "^14.0.0",
    "react": "^18.0.0",
    "react-dom": "^18.0.0",
    "recharts": "^2.8.0"
  },
  "devDependencies": {
    "tailwindcss": "^3.3.0",
    "autoprefixer": "^10.4.0",
    "postcss": "^8.4.0"
  }
}
*/
```
