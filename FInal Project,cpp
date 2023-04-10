#include <iostream>
#include <queue>
#include <random>
#include <vector>

using namespace std;

struct Process {
    int id;           // process ID
    int arrival_time; // arrival time
    int cpu_burst;    // CPU burst time
    int remaining_time; // remaining time
    int waiting_time; // waiting time
    int turnaround_time; // turnaround time
};

void simulate_round_robin(vector<Process> processes, int time_quantum) {
    queue<Process> ready_queue;
    int current_time = 0;
    int num_processes = processes.size();

    // sort processes by arrival time
    sort(processes.begin(), processes.end(),
         [](const Process &a, const Process &b) {
             return a.arrival_time < b.arrival_time;
         });

    int completed = 0;
    while (completed < num_processes) {
        // add newly arrived processes to the ready queue
        for (int i = 0; i < num_processes; i++) {
            if (processes[i].arrival_time <= current_time &&
                processes[i].remaining_time > 0) {
                ready_queue.push(processes[i]);
            }
        }

        if (ready_queue.empty()) {
            // no processes to execute, wait for next process to arrive
            current_time++;
            continue;
        }

        Process current_process = ready_queue.front();
        ready_queue.pop();

        // execute process for time quantum or until completion
        int time_executed = min(time_quantum, current_process.remaining_time);
        current_time += time_executed;
        current_process.remaining_time -= time_executed;

        if (current_process.remaining_time == 0) {
            // process has completed execution
            completed++;
            current_process.waiting_time = current_time -
                                           current_process.arrival_time -
                                           current_process.cpu_burst;
            current_process.turnaround_time = current_time - current_process.arrival_time;
        } else {
            // process still needs to execute, add back to ready queue
            ready_queue.push(current_process);
        }
    }
}

int main() {
    // generate random processes with arrival time and CPU burst time
    const int num_processes = 10;
    const int max_arrival_time = 20;
    const int max_cpu_burst = 10;
    const int time_quantum = 4;
    vector<Process> processes(num_processes);
    random_device rd;
    mt19937 gen(rd());
    uniform_int_distribution<> arrival_time_dist(0, max_arrival_time);
    uniform_int_distribution<> cpu_burst_dist(1, max_cpu_burst);
    for (int i = 0; i < num_processes; i++) {
        processes[i].id = i;
        processes[i].arrival_time = arrival_time_dist(gen);
        processes[i].cpu_burst = cpu_burst_dist(gen);
        processes[i].remaining_time = processes[i].cpu_burst;
    }

    // run simulation
    simulate_round_robin(processes, time_quantum);

    // calculate and print average waiting and turnaround times
    double avg_waiting_time = 0.0, avg_turnaround_time = 0.0;
    for (int i = 0; i < num_processes; i++) {
        avg_waiting_time += processes[i].waiting_time;
        avg_turnaround_time += processes[i].turnaround_time;
    }
    avg_waiting_time /= num_processes;
    avg_turnaround_time /= num_processes;

    cout << "Average waiting time: " << avg_waiting_time << endl;
    cout << "Average turnaround time: " << avg_turnaround_time << endl;

   
