# projects

![Alt text](/relative/path/to/img.jpg?raw=true "Optional Title")

package cmd

import (
	"fmt"
	"io/ioutil"
	"github.com/spf13/cobra"
)

// File paths for flags
var afPath, nfPath string

// Define a command for processing the files
var processCmd = &cobra.Command{
	Use:   "process",
	Short: "Process files with provided paths",
	Run: func(cmd *cobra.Command, args []string) {
		processFiles(afPath, nfPath)  // Call the function that does the file processing
	},
}

// Initialize flags for the command
func init() {
	// Register the process command with the root command
	rootCmd.AddCommand(processCmd)

	// Define flags for the input file paths
	processCmd.PersistentFlags().StringVarP(&afPath, "af", "a", "", "Path to the input file for af")
	processCmd.PersistentFlags().StringVarP(&nfPath, "nf", "n", "", "Path to the input file for nf")
	processCmd.MarkPersistentFlagRequired("af")
	processCmd.MarkPersistentFlagRequired("nf")
}

// The function that processes the files
func processFiles(afPath, nfPath string) {
	afContent, err := ioutil.ReadFile(afPath)
	if err != nil {
		fmt.Printf("Error reading af file: %v\n", err)
		return
	}
	err = ioutil.WriteFile("fileA", afContent, 0644)
	if err != nil {
		fmt.Printf("Error writing to fileA: %v\n", err)
		return
	}
	fmt.Println("Successfully wrote af content to fileA")

	nfContent, err := ioutil.ReadFile(nfPath)
	if err != nil {
		fmt.Printf("Error reading nf file: %v\n", err)
		return
	}
	err = ioutil.WriteFile("fileN", nfContent, 0644)
	if err != nil {
		fmt.Printf("Error writing to fileN: %v\n", err)
		return
	}
	fmt.Println("Successfully wrote nf content to fileN")
}


ROOT.GO:

package cmd

import (
	"fmt"
	"os"

	"github.com/spf13/cobra"
)

var rootCmd = &cobra.Command{
	Use:   "myapp",
	Short: "A simple CLI application",
	Long:  `This is a longer description of the CLI application.`,
}

// Execute is called by main.go to kickstart Cobra
func Execute() {
	if err := rootCmd.Execute(); err != nil {
		fmt.Println(err)
		os.Exit(1)
	}
}


package main

import (
	"myapp/cmd"
)

func main() {
	cmd.Execute()  // Only calls the Execute function from the cmd package
}



CALLING MULTIPLE FUNCS BEYOND MAIN.GO:

package cmd

import (
	"fmt"
	"github.com/spf13/cobra"
)

var afPath, nfPath string

// Define the command that will call multiple functions
var processCmd = &cobra.Command{
	Use:   "process",
	Short: "Process af and nf files",
	Run: func(cmd *cobra.Command, args []string) {
		func1()
		func2()
		func3()
		func4()
	},
}

func init() {
	rootCmd.AddCommand(processCmd)
	processCmd.PersistentFlags().StringVarP(&afPath, "af", "a", "", "Path to the input file for af")
	processCmd.PersistentFlags().StringVarP(&nfPath, "nf", "n", "", "Path to the input file for nf")
	processCmd.MarkPersistentFlagRequired("af")
	processCmd.MarkPersistentFlagRequired("nf")
}

func func1() {
	fmt.Println("Running function 1")
}

func func2() {
	fmt.Println("Running function 2")
}

func func3() {
	fmt.Println("Running function 3")
}

func func4() {
	fmt.Println("Running function 4")
}


