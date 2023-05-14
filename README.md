""" KeeganJohnsonFinalProject.py
By Keegan Johnson """

import tkinter as tk

class Scoreboard(tk.Frame):
    """This defines the Scoreboard class that inherits from the tk.Frame class
    which is a container for other Tkinter widgets."""
    def __init__(self, master=None):
        """This is the constructor method that initializes a Scoreboard object.
        It takes an optional master parameter, which refers to the parent widget."""
        super().__init__(master)
        self.master = master
        self.master.title("Basketball Scoreboard")

        # Initialize scoreboard values
        self.time = tk.StringVar(value="12:00")
        self.quarters = tk.IntVar(value=4)
        self.fouls_home = tk.IntVar(value=0)
        self.fouls_away = tk.IntVar(value=0)
        self.possession = tk.StringVar(value="Home")
        self.score_home = tk.IntVar(value=0)
        self.score_away = tk.IntVar(value=0)
        self.is_timer_running = False

        # Create labels and entry fields for each category
        tk.Label(self, text="Time").grid(row=0, column=0)
        self.time_entry = tk.Entry(self, textvariable=self.time)
        self.time_entry.grid(row=0, column=1)

        tk.Label(self, text="Quarters").grid(row=1, column=0)
        self.quarters_entry = tk.Entry(self, textvariable=self.quarters)
        self.quarters_entry.grid(row=1, column=1)

        tk.Label(self, text="Fouls").grid(row=2, column=0)
        tk.Label(self, text="Home:").grid(row=2, column=1, sticky="E")
        tk.Entry(self, textvariable=self.fouls_home, width=3).grid(row=2, column=2)
        tk.Label(self, text="Away:").grid(row=2, column=3, sticky="E")
        tk.Entry(self, textvariable=self.fouls_away, width=3).grid(row=2, column=4)

        tk.Label(self, text="Possession").grid(row=3, column=0)
        tk.Entry(self, textvariable=self.possession).grid(row=3, column=1)

        tk.Label(self, text="Score").grid(row=4, column=0)
        tk.Label(self, text="Home:").grid(row=4, column=1, sticky="E")
        self.score_home_entry = tk.Entry(self, textvariable=self.score_home, width=3)
        self.score_home_entry.grid(row=4, column=2)
        tk.Button(self, text="+1", command=self.increment_home_score).grid(row=4, column=3)

        tk.Label(self, text="Away:").grid(row=4, column=4, sticky="E")
        self.score_away_entry = tk.Entry(self, textvariable=self.score_away, width=3)
        self.score_away_entry.grid(row=4, column=5)
        tk.Button(self, text="+1", command=self.increment_away_score).grid(row=4, column=6)

        # Create buttons to start and stop the timer
        self.start_button = tk.Button(self, text="Start Timer", command=self.start_timer)
        self.start_button.grid(row=5, column=0, columnspan=2)

        self.stop_button = tk.Button(self, text="Stop Timer", command=self.stop_timer, state=tk.DISABLED)
        self.stop_button.grid(row=5, column=2, columnspan=2)


        # Create buttons for quarters
        tk.Button(self, text="1", command=self.set_quarters_1).grid(row=6, column=0)
        tk.Button(self, text="2", command=self.set_quarters_2).grid(row=6, column=1)
        tk.Button(self, text="3", command=self.set_quarters_3).grid(row=6, column=2)
        tk.Button(self, text="4", command=self.set_quarters_4).grid(row=6, column=3)

        # Create button to update scoreboard
        tk.Button(self, text="Update", command=self.update_scoreboard).grid(row=7, column=2, columnspan=2)

        self.pack()

    def update_scoreboard(self):
        """This method is called when the "Update" button is pressed.
        It retrieves the current values of various scoreboard variables and displays them by printing to the console.
        In a complete implementation, this method could update a physical or graphical scoreboard display. """
        # Update scoreboard based on entered values
        time_str = self.time.get()
        quarters = self.quarters.get()
        fouls_home = self.fouls_home.get()
        fouls_away = self.fouls_away.get()
        possession = self.possession.get()
        score_home = self.score_home.get()
        score_away = self.score_away.get()

        # Update scoreboard display (replace with actual scoreboard update code)
        print(f"Time: {time_str}")
        print(f"Quarters: {quarters}")
        print(f"Fouls - Home: {fouls_home}, Away: {fouls_away}")
        print(f"Possession: {possession}")
        print(f"Score - Home: {score_home}, Away: {score_away}")

    def increment_home_score(self):
        """This method is called when the "+1" button for the home team is pressed.
        It increments the home team's score by 1."""
        # Increment the home score by 1
        self.score_home.set(self.score_home.get() + 1)

    def increment_away_score(self):
        """This method is similar to increment_home_score but increments the away team's score by 1."""
        # Increment the away score by 1
        self.score_away.set(self.score_away.get() + 1)

    def start_timer(self):
        """This method is called when the "Start Timer" button is pressed.
        It starts a countdown timer by repeatedly calling the update_timer method using the after method of the Tkinter Tk object.
        It also updates the state of the start and stop buttons."""
        # Start the timer
        self.is_timer_running = True
        self.start_button.configure(state=tk.DISABLED)
        self.stop_button.configure(state=tk.NORMAL)
        self.update_timer()

    def stop_timer(self):
        """This method is called when the "Stop Timer" button is pressed.
        It stops the timer by updating the is_timer_running attribute and updating the state of the start and stop buttons."""
        # Stop the timer
        self.is_timer_running = False
        self.start_button.configure(state=tk.NORMAL)
        self.stop_button.configure(state=tk.DISABLED)

    def update_timer(self):
        """This method updates the timer display every second while the timer is running.
        It retrieves the current time from the time entry field, converts it to seconds, decrements it by 1,
        and updates the time entry field with the new time.
        If the timer reaches zero, it calls stop_timer to stop the timer."""
        # Update the timer display
        if self.is_timer_running:
            current_time = self.time_entry.get()
            minutes, seconds = current_time.split(":")
            total_seconds = int(minutes) * 60 + int(seconds)

            if total_seconds > 0:
                total_seconds -= 1
                updated_minutes = total_seconds // 60
                updated_seconds = total_seconds % 60
                new_time = f"{updated_minutes:02d}:{updated_seconds:02d}"
                self.time.set(new_time)
                self.master.after(1000, self.update_timer)
            else:
                self.stop_timer()

    def set_quarters_1(self):
        """These methods are called when the respective quarter buttons are pressed. They set the value of the quarters variable to 1"""
        self.quarters.set(1)

    def set_quarters_2(self):
        """These methods are called when the respective quarter buttons are pressed. They set the value of the quarters variable to 2"""
        self.quarters.set(2)

    def set_quarters_3(self):
        """These methods are called when the respective quarter buttons are pressed. They set the value of the quarters variable to 3"""
        self.quarters.set(3)

    def set_quarters_4(self):
        """These methods are called when the respective quarter buttons are pressed. They set the value of the quarters variable to 4"""
        self.quarters.set(4)


class ScoreboardDisplay(tk.Toplevel):
    """This defines the ScoreboardDisplay class that inherits from the tk.Toplevel class, which is a top-level window."""
    def __init__(self, master=None):
        """This is the constructor method that initializes a ScoreboardDisplay object.
        It takes an optional master parameter, which refers to the parent widget."""
        super().__init__(master)
        self.master = master
        self.master.title("Scoreboard Display")

        # Create labels to display the scoreboard
        tk.Label(self, text="Home Score:").grid(row=0, column=0)
        self.home_score_label = tk.Label(self, textvariable=scoreboard.score_home)
        self.home_score_label.grid(row=0, column=1)

        tk.Label(self, text="Away Score:").grid(row=1, column=0)
        self.away_score_label = tk.Label(self, textvariable=scoreboard.score_away)
        self.away_score_label.grid(row=1, column=1)

        tk.Label(self, text="Time:").grid(row=2, column=0)
        self.time_label = tk.Label(self, textvariable=scoreboard.time)
        self.time_label.grid(row=2, column=1)

        tk.Label(self, text="Possession:").grid(row=3, column=0)
        self.possession_label = tk.Label(self, textvariable=scoreboard.possession)
        self.possession_label.grid(row=3, column=1)

        tk.Label(self, text="Quarters:").grid(row=4, column=0)
        self.quarters_label = tk.Label(self, textvariable=scoreboard.quarters)
        self.quarters_label.grid(row=4, column=1)

        tk.Label(self, text="Home Fouls:").grid(row=5, column=0)
        self.home_fouls_label = tk.Label(self, textvariable=scoreboard.fouls_home)
        self.home_fouls_label.grid(row=5, column=1)

        tk.Label(self, text="Away Fouls:").grid(row=6, column=0)
        self.away_fouls_label = tk.Label(self, textvariable=scoreboard.fouls_away)
        self.away_fouls_label.grid(row=6, column=1)

        

root = tk.Tk()
scoreboard = Scoreboard(root)
scoreboard_display = ScoreboardDisplay(root)
scoreboard.mainloop()
